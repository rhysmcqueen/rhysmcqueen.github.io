---
layout: post
title: "Deploying a Monitoring Stack with Prometheus, Grafana, and Alertmanager"
date: 2025-03-10 12:00:00 -0600
tags: 
---

In my homelab, I use a Docker-based monitoring stack powered by Prometheus, Grafana, and Alertmanager. It's deployed and configured with Ansible, making it easy to replicate across environments.

## Stack Overview

- **Prometheus**: collects metrics from targets (including node_exporter on each host)
- **Grafana**: visualizes Prometheus data
- **Alertmanager**: routes alerts via Discord webhook

---

## Network Layout

- Each monitored node runs `prometheus-node-exporter`
- Prometheus lives on `automation1.cal`
- All services run in Docker containers
- Ports exposed: `9090` (Prometheus), `3000` (Grafana), `9093` (Alertmanager)

---

## Ansible Playbooks

### 1. `install-docker.yml`

Installs Docker on all target hosts (if not already present).

```yaml
- name: Install Docker engine
  hosts: docker_hosts
  become: true
  tasks:
    - name: install docker engine
      apt:
        name: 
          - docker-ce 
          - docker-ce-cli 
          - containerd.io
        state: present
```

### 2. prometheus_exporter.yml
Installs node exporter on all monitored hosts.

```yaml
- name: Install and configure Prometheus Node Exporter
  hosts: node-exporter-monitored
  become: true
  tasks:
    - name: Install Node Exporter
      apt:
        name: prometheus-node-exporter
        state: present
```
### 3. grafana-promethus-alertmanager-monitoring-stack.yml
Sets up Prometheus, Grafana, and Alertmanager on automation1.cal.

Prometheus scrapes node_exporters and Alertmanager

Alertmanager alerts are routed via Discord webhook

Grafana is pre-provisioned and persists dashboards

```yaml
- name: Create Prometheus config
  copy:
    dest: /opt/prometheus/config/prometheus.yml
    content: |
      global:
        scrape_interval: 15s
      scrape_configs:
        - job_name: 'prometheus'
          static_configs:
            - targets: ['localhost:9090']
        - job_name: 'alertmanager'
          static_configs:
            - targets: ['localhost:9093']
        - job_name: 'node-exporter'
          static_configs:
            - targets: [
                {% for host in groups['node-exporter-monitored'] %}
                '{{ hostvars[host].ansible_host }}:9100'{% if not loop.last %},{% endif %}
                {% endfor %}
              ]
```
You can also define alert rules:

```yaml
- name: Create alert rules
  copy:
    dest: /opt/prometheus/config/alert_rules.yml
    content: |
      groups:
        - name: example
          rules:
            - alert: InstanceDown
              expr: up == 0
              for: 1m
              labels:
                severity: critical
              annotations:
                summary: "Instance {{ $labels.instance }} down"
```
And Alertmanager configuration (Discord):

```yaml
- name: Create Alertmanager config
  copy:
    dest: /opt/alertmanager/config/alertmanager.yml
    content: |
      global:
        resolve_timeout: 5m
      route:
        receiver: 'discord'
      receivers:
        - name: 'discord'
          webhook_configs:
            - url: '{{ discord_webhook_url }}'
              send_resolved: true
```
### Inventory
The Ansible inventory groups hosts like this:

```yaml
node-exporter-monitored:
  hosts:
    vm-1.cal:
    vm-2.cal:
    gameserver-01.cal:

docker_hosts:
  children:
    node-exporter-monitored:
```
### Final Notes
Once deployed, Grafana is available on port 3000. Log in with the default admin credentials and start creating dashboards. Prometheus scrapes metrics automatically, and alerts trigger Discord webhooks.

📁 Full playbooks and config:
https://github.com/rhysmcqueen/Learning/tree/main/Ansible-Examples/playbooks

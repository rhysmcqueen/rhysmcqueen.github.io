---
title: Homelab Introduction
date: 2024-12-1 12:00:00 -700
categories: [Infrastructure,Networking,Hardware,SelfHost]
tags: [homelab,networking,selfhost]     # TAG names should always be lowercase
---

# Homelab Intro
My Homelab has been growing since 2018 which was the first time I bought a used PC to solely be ran as a Server. It started with a Plex Server and a Game server. Now we are still running Plex servers and game servers BUT with so much more. At this point calling it a homelab is debatable. It is spread accross 2 homes (Calgary and in Ontario) as well as having a few Cloud nodes thrown in the mix as well (Oracle and AWS).

## How it started
Learning networking and cableing started in my parents house when I was always tasked with "Fixing" the Wifi. I usually had to fix it after I broke it and didnt tell anyone. (*Sorry Bell I blamed you for 1 or 2 things that were not deserved.*)
I grew to running cables throughout my house by drilling a 2 1/4'' coduit through the foundation and running that up to the attic where I can then drop cables into the wall from the top. I cabled every bedroom without my parents having a clue what was going on (untill the hammer drill came out to drill into the foundation). I then wired it to a patch panel and small network rack I got off ebay.


![Burl-Homelab-1](/assets/2024-12-1-intro-homelab/burl-homelab-1.jpg){: w="300" h="100" }
![Burl-homelab-2](/assets/2024-12-1-intro-homelab/burl-homelab-2.jpg){: w="300" h="100" }
![Burl-homelab-3](/assets/2024-12-1-intro-homelab/burl-homelab-3.png){: w="200" h="100" }

## HomeLab Growth
After my Family moved to a new house I had to rethink the networking goals. I was now halfway through college and had a little more knowledge and goals for my homelab. I started with getting a fullsize rack and enclosing it inside a closet. I terminated all the ethernet and speaker wires that were ran throughout the house and bought some legitimet IT hardware. A 10+ year old Dell R710 Server and a Juniper EX3300-48p switch. This is how I learned alot! I overloaded that R710 with way too many vms and got into Hypervisors, NAS software and more and more. That workhorse of a host was only retired a few months ago in Sepetember (2024). That is a 15 year lifespan on that host from when it was released.
I got into softwares like:
* Proxmox (Hypervisor)
* TrueNAS(ZFS NAS Software)
* Docker
* Kubernetes
* Ansiable
* Terraform
* Chat Bots (Discord)
* Reverse Proxies
* DNS
* Game Servers
* And MORE AND MORE

![Ont-homelab-1](/assets/2024-12-1-intro-homelab/ont-homelab-1.jpg){: w="300" h="100" }
![Ont-homelab-2](/assets/2024-12-1-intro-homelab/ont-homelab-2.jpg){: w="300" h="100" }
![Ont-homelab-3](/assets/2024-12-1-intro-homelab/ont-homelab-3.jpg){: w="300" h="100" }
![Ont-homelab-4](/assets/2024-12-1-intro-homelab/ont-homelab-4.jpg){: w="300" h="100" }
![Ont-homelab-5](/assets/2024-12-1-intro-homelab/ont-homelab-5.jpg){: w="300" h="100" }

## Where are we now?

Well Now I live in Calgary where I have the space to buy ANOTHER rack and fill it with even more servers. I am now running a Highly availiable proxmox cluster backed by a CEPH filesystem. All of it is backed up by a Primary and Secondary UPS system with full hight vertical PDUS at the back. Is any of this necessary.... NO but whats the fun in that! With all of this I am able to do multi region services that will auto failover between my Ontario homelab and my Calgary homelab. I mix in some cloud instances for learning as well as a external monitoring system that alerts me of any issues with the public facing services I am running. I ran 2 fiber cables underground to connect my detatched garage (Where my Server Rack is) to the house where I run a single switch for all the devices inside the house. All switches and Servers are running on 40gbps links. 

![Cal-homelab-1](/assets/2024-12-1-intro-homelab/cal-homelab-1.jpg){: w="300" h="100" }

## Where next?
I dont know. Right now I am still trying to configure my Calgary homelab to take over all the roles from the Ontario site so I can re-deploy and re-image all those servers to start fresh there. A lot of technical debt there. As well as making all my new deployments work with Terraform and Asniable so I can copy and past configs across the two "Regions"
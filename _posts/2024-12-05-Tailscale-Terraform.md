---
layout: post
title: "Tailscale and Terraform!"
date: 2024-12-02 09:00:00 -0500
---

### What Does Terraform Do for Tailscale?
Managing Tailscale at a large scale without manual intervention is critical for efficiency. Manually setting tags, approving devices, configuring subnets, and creating authentication keys would make scaling Tailscale in a large environment nearly impossible.

As demonstrated in [AWS Multi-Site](https://blog.mcqueenlab.net/posts/aws-multi-vpc/), Terraform enables rapid deployment of an entire multi-VPC infrastructure in seconds. Similarly, Terraform can streamline and automate Tailscale configurations, making it both efficient and scalable.

---

### Learning Steps
I started by exploring the [Tailscale Terraform Provider](https://registry.terraform.io/providers/tailscale/tailscale/latest/docs). To familiarize myself with its capabilities, I deployed each module individually. Since I was working with a fresh Tailscale account, my first goal was to manage my account settings programmatically. Below is the code that defines my Tailnet settings using Terraform:

```hcl
variable "tailscale_api_key" {}
variable "tailnet" {}
variable "discord_webhook" {}

terraform {
  required_providers {
    tailscale = {
      source = "tailscale/tailscale"
      version = "0.17.2"
    }
  }
}

provider "tailscale" {
  api_key = var.tailscale_api_key
  tailnet = var.tailnet
}

resource "tailscale_tailnet_settings" "sample_tailnet_settings" {
  devices_approval_on                         = true
  devices_auto_updates_on                     = true
  devices_key_duration_days                   = 7
  users_approval_on                           = true
  users_role_allowed_to_join_external_tailnet = "member"
  posture_identity_collection_on              = true
  # network_flow_logging_on                     = true    # NEED PREMIUM/PAID PLAN
  # regional_routing_on                         = true    # NEED PREMIUM/PAID PLAN
}

resource "tailscale_contacts" "sample_contacts" {
  account {
    email = "learn.mcqueenlab@gmail.com"
  }

  support {
    email = "learn.mcqueenlab@gmail.com"
  }

  security {
    email = "learn.mcqueenlab@gmail.com"
  }
}

resource "tailscale_webhook" "discord_webhook" {
  endpoint_url  = var.discord_webhook
  provider_type = "discord"
  subscriptions = [
    "nodeCreated",
    "userDeleted",
    "nodeNeedsApproval",
    "userNeedsApproval",
    "subnetIPForwardingNotEnabled"
  ]
}
```
With this setup, all Tailnet settings are managed as code. I also automated daily Terraform runs that notify a Discord channel if any changes are detected.
### Additional Configurations
Next, I explored creating authentication keys, approving devices, and managing subnet routes and ACLs via Terraform. Since these topics involve significant details, the relevant examples are available in my [GitHub repository](https://github.com/rhysmcqueen/Learning/tree/main/Software-Examples/Tailscale-Examples/Tailscale-Terraform).

### Final Thoughts
I’m thrilled with the time spent diving into the Tailscale Terraform Provider. It’s a first-party solution with extensive functionality, making Tailscale management seamless and scalable. While I still need to experiment with data source modules to extract information from the Tailnet, I’ve already achieved most of what I set out to do. Terraform has truly transformed how I manage Tailscale!
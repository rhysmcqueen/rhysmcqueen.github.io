---
layout: post
title: "Multi VPC AWS Cluster"
date: 2024-12-02 09:00:00 -0500
---
### Backstory
I have used terraform before mainly in College where I had a amazing proffessor who introduced us to terraform as a resource for his class. He used the configs as a way to setup a cloud config for labs on security testing. This was great as a few classes I have done in college we would spend 2 entire classes just setting up envitoments in Virtual box or others! From there I never really kept up with my Terraform learning. I then decided when building my new homelab I wanted to build it using IAC techniques so I decided to relearn Terraform. I started with Relearning the AWS provider since that is what I was most familiar with.

In this example I show how to make a configarable cloud state that can have multiple segerated regions(vpc's) and run a webpage on them. 

### How did I start?
Well if the goal is to setup 3 similar vpcs all with subnets, route-tables,ec2 instances and more. You should start with just 1.

GitHub Repo: [Terraform-AWS-Single-Instance](https://github.com/rhysmcqueen/Learning/tree/main/Terraform-Example/AWS-Single-EC2-Instance)

![aws-single-instance](https://github.com/rhysmcqueen/Learning/raw/main/Terraform-Example/AWS-Single-EC2-Instance/Diagrams/Single-Instance.png)

From there I moved onto the full project of a unlimted (but limited) amount of VPC setups all with their own webpage.

### Multi-AWS-VPC
The stugles I had with this one were vast. From as simple as remembering how to set variables for a array of city names to ensuring all 3 vpcs has unique IPs and subnets. 

GitHub Repo: [Terraform-AWS-Mutli-VPC](https://github.com/rhysmcqueen/Learning/tree/main/Terraform-Example/AWS-Mutli-VPC)

![aws-single-instance](https://github.com/rhysmcqueen/Learning/raw/main/Terraform-Example/AWS-Mutli-VPC/Diagrams/Multi_VPC_Diagram.drawio.png)

After the terraform apply it takes only about 30 seconds to full deploy each vpc.
In total everything that is added is:
* VPC
* Subnet
* Internet Gateway
* Route Table
* Route Table association
* Security Groups
* AWS EC2-Keypair
* EC2 T3.nano Instance

```bash
Apply complete! Resources: 26 added, 0 changed, 0 destroyed.

Outputs:

instance_names_and_ips = 
Calgary-instance: 40.176.84.253
Toronto-instance: 40.177.108.31
Vancouver-instance: 40.176.122.242
```

Tailscale Admin Console:
![tailscale](/assets/2024-12-02-aws-multi-vpc/tailscale-Dashboard.png)
As you can see it has already accepted the routes that the subnet router is creating. No manual configuration needed at any step!

AWS Console:
![tailscale](/assets/2024-12-02-aws-multi-vpc/aws-console.png)

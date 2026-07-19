---
title: "Blog 2"
date: 2026-05-04
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# The Trio of AWS Services in the Architecture

> **Original Post:** [AWS Study Group - FCAJ](https://www.facebook.com/photo?fbid=1541622047518617&set=gm.2202005003897793&idorvanity=660548818043427&locale=vi_VN)

![Amazon VPC, EC2 và Amazon EFS](/images/3-BlogsTranslated/Blog2_photo.jpg)

When designing infrastructure for systems that need to run multiple web servers (such as WordPress Clusters, file processing systems, or enterprise applications), I often encounter a classic challenge: **How can virtual EC2 servers located in different Availability Zones (AZs) concurrently access, read, and write to the same shared data directory without drift or loss of synchronization?**

Typically, when building a system, we have to solve two distinct challenges:
* **Compute:** EC2 servers need to scale flexibly and operate across multiple zones to ensure High Availability.
* **Storage:** Data must be centralized, consistent, and allow hundreds of servers to write and read simultaneously without conflict.

Normally, we often use the default EBS (Elastic Block Store) volumes for EC2. The pain point is that EBS can only be attached to one EC2 instance at a time (except for certain special EBS types that support Multi-Attach, which are still limited to the same AZ). If we want to share files across AZs, we have to manually set up rsync cronjobs or configure complex systems like GlusterFS or NFS manually. Doing so is extremely exhausting and highly error-prone (you might end up corrupting your data before you even know it).

That is why AWS introduced **Amazon Elastic File System (EFS)** - a shared file system service operating at the Regional level, utilizing the fully managed NFSv4 protocol.

## How the system works through the architecture diagram
Looking at the design diagram above, you can see how this trio of services coordinates extremely smoothly:
* **The Protective Outer Ring - Amazon VPC:** Acts as a virtual private network that isolates all resources. The system is split into multiple subnets across three independent Availability Zones: `us-west-2a`,` us-west-2b`, and `us-west-2c`.
* **The Processing Layer - Amazon EC2:** To ensure the application doesn't go down when an AZ experiences an issue, the EC2 servers are widely distributed:
* In AZ `us-west-2a`: There are two EC2 servers running in parallel (IPs `10.0.1.30` and `10.0.1.31`) within subnet `10.0.1.0/24`.
* In AZ `us-west-2c`: There is one EC2 server (IP `10.0.4.1`) within subnet `10.0.4.0/24`.
* **The Shared Storage Layer - Amazon EFS:** Located at the center and connected to the AZs through a concept called a **Mount Target**. Each AZ has a Mount Target with its own local IP address:
* AZ `us-west-2a` uses Mount Target IP `10.0.1.32`.
* AZ `us-west-2a` uses Mount Target IP `10.0.2.45` (with subnet `10.0.2.0/24` pre-configured to be ready for future EC2 Auto Scaling).
* AZ `us-west-2c` uses Mount Target IP `10.0.3.15`.

## Extremely smart network design point (SecOps)
A point I find quite interesting in this diagram is how the subnetting is designed in zone `us-west-2c`.

The EC2 server resides in subnet `10.0.4.0/24`, but the EFS Mount Target is placed in a completely separate subnet `10.0.3.0/24`. This partitioning allows us to configure extremely tight Security Groups: only allowing traffic from the subnet containing the EC2 instances to enter the subnet containing the EFS Mount Target via the traditional NFS port (Port 2049). This minimizes the risk of unrelated network resources snooping or accessing the shared data store without authorization.

## Key benefits of this model:

* **Complete separation of Compute and Storage layers:** This ensures that scaling up or down EC2 servers does not affect data safety. You can terminate or add EC2 instances without worrying about migrating or copying files.
* **No worries about running out of capacity:**Unlike EBS, which requires pre-configured fixed capacity, EFS automatically scales its capacity (Elastic) based on the actual files you upload. As you write more, it expands; as you delete, it automatically shrinks, optimizing pay-per-use costs. 
* **Instant synchronization(Zero rsync):** Any file write operation from a server in AZ `us-west-2a` will immediately reflect on the server in AZ `us-west-2c` without needing to configure any sync commands.
* **Extremely high availability:**  Data stored on EFS is automatically replicated and distributed across multiple AZs by AWS by default.

In my opinion, the combination of Amazon VPC, EC2, and EFS is currently the most standard and reliable blueprint when you need to build multi-instance systems that require shared file resources on AWS..

**References:**
* **Amazon EFS User Guide:** <https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html>
* **How to mount an EFS file system on an EC2 instance:** <https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html>
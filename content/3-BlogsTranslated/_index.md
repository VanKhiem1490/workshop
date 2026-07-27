---
title: "Translated Blogs"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 -Overview Architecture of AI Image Upscaling System on Cloud](3.1-Blog1/)
This post provides a comprehensive analysis of the architecture for an AI image upscaling service (Real-ESRGAN) and traditional algorithms (LANCZOS) optimized on AWS GPUs. Covering layers from Frontend, API, AI Processing to Queue (SQS) and Storage, it highlights core design decisions for decoupling workloads, optimizing GPU memory (FP16, Tile-based processing), and implementing effective cost management strategies.

###  [Blog 2 - The Ultimate AWS Trio: Combining VPC, EC2, and Amazon EFS](3.2-Blog2/)
This post analyzes the architectural model combining three core services—Amazon VPC, EC2, and Amazon Elastic File System (EFS)—to create a secure, high-availability, and fault-tolerant infrastructure. It delves into multi-AZ subnet partitioning and the centralized file-sharing mechanism via Mount Targets, ensuring data consistency across the EC2 cluster.

###  [Blog 3 -Serverless Architecture on AWS](3.3-Blog3/)
This post introduces a Serverless architecture on AWS combining Amazon S3, API Gateway, AWS Lambda, and DynamoDB to build a book management web application. It analyzes in detail the data processing flow via API Gateway, as well as the asynchronous image resizing and optimization mechanism triggered by S3 Events, helping the system scale flexibly and minimize operational costs.


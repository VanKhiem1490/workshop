---
title: "Blog 1"
date: 2026-05-04
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# AI Image Upscaling Architecture

> **Original Post:** [AWS Study Group - FCAJ](https://www.facebook.com/photo?fbid=1536588114688677&set=gm.2196040641160896&idorvanity=660548818043427&locale=vi_VN)

![AI Image Upscaling Architecture](/images/3-BlogsTranslated/Blog1_photo.jpg)

Hi everyone! Today, I want to share with you the architectural design of an image processing system that I have recently researched and consolidated.

In real-world projects, integrating AI models to upscale image quality (such as Real-ESRGAN) is extremely popular. However, running these heavy deep learning models in production is always a challenging puzzle: How do we ensure sharp image quality while optimizing expensive GPU resources and preventing server crashes during peak traffic?

Below is the high-level design for an AI Image Upscaling API service that combines AI and traditional processing methods (LANCZOS), optimized to run smoothly on the Cloud.

##  Detailed Processing Flow
Here is the data flow through the step-by-step numbers in the architecture diagram:

1. **Bước 1:** The user sends a request from the client, passing through **AWS WAF** to filter out malicious application-layer traffic.
2. **Bước 2:** Clean requests are forwarded to **Amazon CloudFront (CDN)** to optimize the distribution speed of static content.
3. **Bước 3:** CloudFront interacts directly with the Next.js static frontend web application hosted on **AWS Amplify**.
4. **Bước 4:** The frontend uses **Amazon Cognito** to authenticate users and issue secure JWT Tokens for subsequent tasks.
5. **Bước 5:** The user sends an image processing request along with the authentication token to **Application Load Balancer (ALB)**.
6. **Bước 6:** The ALB routes these API requests to the **Amazon EC2** cluster running the **FastAPI Backend** application.
7. **Bước 7:** The FastAPI backend receives the request, validates the image file, and simultaneously triggers processing flows: records metadata, pushes the job to a queue, and triggers image analysis.
8. **Bước 8:** The job message describing the task is pushed to an asynchronous **Amazon SQS** queue to smooth out traffic load and prevent API system congestion.
9. **Bước 9:** In parallel, **Amazon Rekognition** performs a quick analysis of the original image (such as face detection) and sends the results to the **AWS SageMaker** orchestrator.
10. **Bước 10:** **AWS SageMaker** acts as a "Smart Processor". Based on the results from Rekognition, it automatically routes the task to the worker with the most optimal AI model:
    * Standard landscape images -> Routed to **Deep Learning AMI (Real-ESRGAN Upscale)**.
    * Portrait images containing faces -> Routed to **Apache MXNet (GFPGAN Face Enhance)**.
    * Old, blurry, or scratched images -> Routed to **TensorFlow on AWS (CodeFormer Restore)**.
11. **Bước 11:** Once the AI workers complete processing, the high-quality output image file is saved directly to **Amazon S3 (Object Storage)**.
12. **Bước 12:** This is the direct upload flow of the user's original image from the FastAPI Backend to **Amazon S3** storage as soon as the system receives the request.
13. **Bước 13:** The FastAPI backend continuously updates the job status (Pending, Processing, Completed, Failed) in the **Amazon DynamoDB** database.
14. **Bước 14:** Both the Backend API and the AI processing workers access **AWS Secrets Manager** to retrieve database credentials and API keys securely without hardcoding.
15. **Bước 15:** SageMaker triggers the serverless **AWS Lambda** service to perform lightweight post-processing tasks (such as generating thumbnails or sending webhook notifications of task completion to the user).
16. **Bước 16:** The FastAPI Backend reads/writes hot data (such as Job status or S3 pre-signed URLs) via **Amazon ElastiCache** to minimize the load on DynamoDB and speed up response times for the Frontend.

## Core Design Decisions
* **Complete separation of API and AI Workloads:** The API layer only requires small, inexpensive, and fast-scaling CPU instances. Meanwhile, the AI Workload runs independently on expensive GPU instances and only scales based on the actual number of jobs in the SQS queue.
* **Queue-driven Processing:** Since AI processing is time-consuming, pushing tasks to a queue helps avoid connection timeouts, smooths out sudden traffic spikes, and increases the overall resilience of the system.
* **Preloading Models:** The system preloads common AI models into GPU memory as soon as the worker starts up to minimize latency for the initial requests (avoiding the "cold start" phenomenon).
* **Optimizing GPU Memory with FP16 and Tile-Based Processing:**
* *FP16 (Half-precision):* Using 16-bit floating-point numbers reduces the required GPU memory by half with virtually no loss in output image quality.

## Evaluation: Advantages, Risks, and Proposed Solutions

### Outstanding Advantages
* **Flexibility & Ease of Deployment:** The architecture is designed to be cloud-native, making it extremely suitable for rapid deployment on the AWS cloud environment.
* **Cost Optimization:** By decoupling the roles of each layer, we only pay for expensive GPU resources when there is an actual need for image processing.
* **Enhanced User Experience (UX):** Thanks to the asynchronous queue mechanism, real-time status updates using cache, and detailed metadata returned with the image.

### Risks and Practical Improvement Proposals

| Potential Risks | Proposed Solutions |
| :--- | :--- |
| **GPU Out of Memory (OOM) errors on excessively large images** | Automatically detect input image dimensions. If they exceed safe thresholds, trigger tile-based processing and switch to FP16 format. Set up an automatic retry logic using lower resources upon failure. |
| **Extremely high GPU operational costs** | Apply Auto Scaling to AI workers based on the queue size. During off-peak hours, automatically scale down the number of GPU workers to 0 or 1. Consider using **AWS Spot Instances** to save up to 70-90% in costs. |
| **Latency when transferring large image files over API** | Instead of forcing the client to stream the image file directly through the API server, use a **S3 Pre-signed URL** mechanism. The client will upload the image directly to S3 and download the output image directly from S3, freeing up bandwidth for the API server.|

## Conclusion
This architecture strikes a reasonable balance between image processing quality and practical operational efficiency in a cloud environment. The system not only effectively addresses scaling challenges but also outlines a very clear roadmap for cost and performance optimization for businesses.

What do you think of this design? Is there any point that needs further optimization? Let's discuss below! Happy coding, everyone!


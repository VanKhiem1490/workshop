---
title: "Blog 3"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Serverless Architecture on AWS

> **Original Post:** [AWS Study Group - FCAJ](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2190041575094136)

![Serverless Architecture on AWS](/images/3-BlogsTranslated/Blog3_photo.jpg)

Hello AWS Study Group VN folks!

When starting to migrate applications from traditional server models to the Cloud, "Serverless" is always the most mentioned keyword. How can we build a system capable of infinite automatic scaling, smooth asynchronous image processing, and especially, zero operational cost when there are no users?

Today, I want to share with you a highly intuitive and practical Serverless architecture through the book management system (Books Service) below.

## 1. Why Serverless? Server operations worries disappear
Normally when deploying traditional web applications, we have to deal with headaches like:

* What size of server (EC2) should we rent? Will it handle peak-hour traffic and avoid waste at night?

* Who will take care of OS patching, security, and load balancing configuration?

* How to handle large uploaded image files to avoid bandwidth congestion and server crashes?

Serverless architecture (No servers) solves all of these worries. The entire infrastructure from servers, memory, to networking is fully managed automatically by AWS services. You only need to write code (Business Logic) and pay precisely for each millisecond your code executes.

## 2. Deep dive into the system's data processing flow
Let's deep dive into how this book management system operates when users interact:

### A. Loading the User Interface (Static Web Hosting)
The entire front-end source code (static HTML, CSS, JS) is packaged and stored on an **Amazon S3 Bucket (host static web)**. When a user visits, S3 directly distributes this interface to the browser. This approach gives the website extremely fast page load speeds without wasting money on running a Node.js or Apache server 24/7.

### B. Business Logic Processing (REST API)
When users perform actions on the interface, the browser sends API requests to **Amazon API Gateway**. This service acts as an intelligent "gatekeeper and router", automatically forwarding requests to the corresponding **AWS Lambda** functions to process business logic:

* **View books list (GET /books):** The request is forwarded to the `list_books` function. This function queries data from the **DynamoDB (Books table)** to retrieve the list of books and return it to the user.

* **Add new book (POST /books):** Forwarded to the `create_book` function. This Lambda will record the new book information and simultaneously upload the original cover image file uploaded by the user to the **S3 Bucket (store raw file).**
**
* Delete book (DELETE /books/:id):** Triggers the `delete_book` function to delete book information in the database, and cleans up the related cover image files in the **S3 Bucket (store resized file).**

### C. Asynchronous Image Processing (Event-driven Image Resizing)
One of the brightest highlights of this architecture is the **Event-driven mechanism** to optimize images:

1. When the original cover image is uploaded to the **S3 Bucket (store raw file)**, an event (S3 Event Trigger) is automatically emitted.

2. This event triggers the **AWS Lambda (resize image)** function to run completely independently and asynchronously.

3. The Lambda compresses the image, resizes it to a standard web aspect ratio, and then writes this lightweight, processed image file to the **S3 Bucket (store resized file)**.


## 3. Outstanding benefits of this Serverless model

| Criteria | Serverless Advantages |
| :--- | :--- |
| **Automatic Scaling (Scalability)** | Whether there is 1 or 100,000 concurrent users, API Gateway and AWS Lambda will automatically scale up to handle every request and automatically scale down to 0 when there are no users. |
| **Maximum Cost Optimization** | No fixed monthly maintenance fees for servers. You only pay when Lambda functions actually run (with a highly generous Free Tier of up to 1 million requests/month). |
| **Durability & Extremely Low Maintenance** | The entire system runs on fully managed services. AWS is responsible for maintaining hardware, OS, and ensuring the system always runs stably.|

## Conclusion
The Serverless model combining S3, API Gateway, Lambda, and DynamoDB is the optimal choice for modern application projects. It helps startups or small development teams easily roll out products to the market extremely quickly without having to worry about complex infrastructure management and operations.

Are you applying Serverless to your projects? Share your experience with cover image processing or DynamoDB optimization below! Happy coding, everyone!
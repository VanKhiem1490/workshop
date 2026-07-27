---
title: "Các bài blogs đã dịch"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - Kiến trúc tổng quan hệ thống AI Image Upscaling trên hạ tầng Cloud](3.1-Blog1/)
Bài viết phân tích toàn diện kiến trúc dịch vụ nâng cấp ảnh AI (Real-ESRGAN) và thuật toán truyền thống (LANCZOS) tối ưu trên GPU AWS. Qua các tầng xử lý từ Frontend, API, AI Processing đến Queue (SQS) và Storage, bài viết chia sẻ các quyết định thiết kế cốt lõi giúp tách biệt tải xử lý, tối ưu bộ nhớ GPU (FP16, Tile-based) và chiến lược quản trị chi phí vận hành hiệu quả.

###  [Blog 2 - Bộ ba dịch vụ AWS đỉnh cao: Kết hợp VPC, EC2 và Amazon EFS](3.2-Blog2/)
Bài viết phân tích mô hình kết hợp ba dịch vụ cốt lõi Amazon VPC, EC2 và Amazon Elastic File System (EFS) để tạo nên hạ tầng an toàn, có tính sẵn sàng cao (High Availability) và khả năng chịu lỗi vượt trội. Bài viết đi sâu vào giải pháp phân chia Subnet đa vùng (Multi-AZ) và cơ chế chia sẻ tệp tin tập trung qua Mount Target giúp cụm máy chủ EC2 luôn đồng nhất dữ liệu.

###  [Blog 3 - Kiến trúc Serverless trên AWS](3.3-Blog3/)
Bài viết giới thiệu kiến trúc Serverless trên AWS kết hợp Amazon S3, API Gateway, AWS Lambda và DynamoDB để xây dựng ứng dụng web quản lý sách. Bài viết phân tích chi tiết luồng xử lý dữ liệu qua API Gateway, cơ chế tự động hóa tối ưu và thay đổi kích thước ảnh bất đồng bộ thông qua S3 Event, giúp hệ thống tự động mở rộng quy mô linh hoạt và giảm thiểu tối đa chi phí vận hành.

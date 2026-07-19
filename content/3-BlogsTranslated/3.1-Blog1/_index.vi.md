---
title: "Blog 1"
date: 2026-05-04
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Kiến trúc AI Image Upscaling

> **Bài viết gốc:** [AWS Study Group - FCAJ](https://www.facebook.com/photo?fbid=1536588114688677&set=gm.2196040641160896&idorvanity=660548818043427&locale=vi_VN)

![AI Image Upscaling Architecture](/images/3-BlogsTranslated/Blog1_photo.jpg)

Chào mọi người! Hôm nay mình muốn chia sẻ với các bạn về thiết kế kiến trúc của một hệ thống xử lý hình ảnh mà mình vừa tìm hiểu và tổng hợp được.

Trong các dự án thực tế, việc tích hợp các mô hình AI để nâng cấp chất lượng ảnh (Image Upscaling) như Real-ESRGAN đang cực kỳ phổ biến. Tuy nhiên, việc chạy các model deep learning nặng đô này trên production luôn là một bài toán hóc búa: Làm sao để vừa đảm bảo chất lượng ảnh sắc nét, vừa tối ưu hóa được tài nguyên GPU đắt đỏ và không làm sập server khi có lượng truy cập lớn?

Dưới đây là bản thiết kế tổng quan cho dịch vụ AI Image Upscaling API kết hợp cả AI và các phương pháp xử lý truyền thống (LANCZOS), được tối ưu hóa để vận hành mượt mà trên môi trường Cloud.

##  Quy trình xử lý chi tiết
Dưới đây là luồng đi của dữ liệu qua các bước được đánh số chi tiết trên sơ đồ kiến trúc:

1. **Bước 1:** Người dùng gửi yêu cầu từ client, đi qua hệ thống tường lửa **AWS WAF** để lọc bỏ các lưu lượng độc hại ở tầng ứng dụng.
2. **Bước 2:** Các yêu cầu sạch được chuyển đến **Amazon CloudFront (CDN)** để tối ưu hóa tốc độ phân phối nội dung tĩnh.
3. **Bước 3:** CloudFront tương tác trực tiếp với ứng dụng web tĩnh Next.js Frontend được host trên **AWS Amplify**.
4. **Bước 4:** Frontend sử dụng **Amazon Cognito** để xác thực người dùng và cấp JWT Token an toàn cho các tác vụ tiếp theo.
5. **Bước 5:** Người dùng gửi yêu cầu xử lý ảnh kèm theo token xác thực đến **Application Load Balancer (ALB)**.
6. **Bước 6:** ALB định tuyến các API request này đến cụm máy chủ **Amazon EC2** đang chạy ứng dụng **FastAPI Backend**.
7. **Bước 7:** FastAPI tiếp nhận yêu cầu, kiểm tra tính hợp lệ của file ảnh và đồng thời kích hoạt các luồng xử lý: ghi nhận metadata, đẩy job vào hàng đợi và kích hoạt phân tích hình ảnh.
8. **Bước 8:** Thông điệp mô tả công việc (job message) được đẩy vào hàng đợi bất đồng bộ **Amazon SQS** để điều hòa tải, tránh gây nghẽn hệ thống API.
9. **Bước 9:** Song song đó, **Amazon Rekognition** thực hiện phân tích nhanh ảnh gốc (như phát hiện khuôn mặt) và gửi kết quả về cho bộ điều phối **AWS SageMaker**.
10. **Bước 10:** **AWS SageMaker** đóng vai trò là "Smart Processor". Dựa vào kết quả từ Rekognition, nó sẽ tự động định tuyến tác vụ đến worker có mô hình AI tối ưu nhất:
    * Ảnh phong cảnh thông thường -> Chuyển đến **Deep Learning AMI (Real-ESRGAN Upscale)**.
    * Ảnh chân dung có chứa khuôn mặt -> Chuyển qua **Apache MXNet (GFPGAN Face Enhance)**.
    * Ảnh cũ, mờ xước -> Chuyển đến **TensorFlow on AWS (CodeFormer Restore)**.
11. **Bước 11:** Sau khi các worker AI xử lý xong, tệp tin ảnh đầu ra chất lượng cao sẽ được lưu thẳng vào **Amazon S3 (Object Storage)**.
12. **Bước 12:** Đây là luồng tải trực tiếp ảnh gốc của người dùng từ FastAPI Backend lên lưu trữ **Amazon S3** ngay khi hệ thống nhận được request.
13. **Bước 13:** FastAPI cập nhật liên tục trạng thái công việc (Pending, Processing, Completed, Failed) vào cơ sở dữ liệu **Amazon DynamoDB**.
14. **Bước 14:** Cả Backend API và các Worker xử lý AI đều truy cập **AWS Secrets Manager** để lấy database credentials và API keys một cách bảo mật mà không cần hardcode.
15. **Bước 15:** SageMaker kích hoạt dịch vụ serverless **AWS Lambda** để thực hiện các tác vụ hậu kỳ nhẹ (như tạo ảnh thu nhỏ - thumbnail hoặc gửi webhook thông báo hoàn thành tác vụ cho người dùng).
16. **Bước 16:** FastAPI Backend đọc/ghi các dữ liệu nóng (trạng thái Job hoặc pre-signed URL của S3) thông qua **Amazon ElastiCache** để giảm tải tối đa cho DynamoDB và tăng tốc phản hồi cho Frontend.

## Những quyết định thiết kế cốt lõi

* **Tách biệt hoàn toàn API và AI Workload:** API layer chỉ cần các instance CPU nhỏ, rẻ tiền và scale cực nhanh. Trong khi đó, AI Workload chạy độc lập trên các GPU instance đắt đỏ và chỉ scale dựa trên số lượng công việc thực tế trong hàng đợi SQS.
* **Queue-driven Processing (Xử lý hướng hàng đợi):** Vì xử lý AI mất nhiều thời gian, việc đẩy tác vụ vào Queue giúp tránh lỗi timeout kết nối, "làm mềm" các đợt traffic tăng đột biến (spike load) và tăng độ bền bỉ cho toàn bộ hệ thống.
* **Tải trước mô hình (Preload Model):** Hệ thống tải sẵn các model AI phổ biến vào bộ nhớ GPU ngay khi khởi động worker để giảm thiểu tối đa độ trễ cho những request đầu tiên (tránh hiện tượng "cold start").
* **Tối ưu bộ nhớ GPU bằng FP16 và Tile-based:**
  * *FP16 (Half-precision):* Sử dụng số thực 16-bit giúp giảm một nửa dung lượng bộ nhớ GPU cần thiết mà hầu như không làm giảm chất lượng ảnh đầu ra.
  * *Tile-based processing:* Chia nhỏ các bức ảnh có độ phân giải quá lớn thành các mảnh (tiles) nhỏ để xử lý tuần tự hoặc song song, tránh tình trạng sập worker do tràn bộ nhớ GPU (Out of Memory - OOM).

## Đánh giá: Ưu điểm, Rủi ro và Giải pháp đề xuất

### Ưu điểm nổi bật
* **Linh hoạt & Dễ triển khai:** Kiến trúc được thiết kế chuẩn cloud-native, cực kỳ phù hợp để triển khai nhanh trên môi trường đám mây AWS.
* **Tối ưu hóa chi phí tốt:** Nhờ tách biệt vai trò của từng layer, chúng ta chỉ phải trả tiền cho GPU đắt đỏ khi thực sự có nhu cầu xử lý ảnh.
* **Nâng cao trải nghiệm người dùng (UX):** Nhờ cơ chế hàng đợi bất đồng bộ, cập nhật trạng thái thời gian thực bằng bộ nhớ đệm và các thông tin metadata chi tiết trả về kèm ảnh.

### Rủi ro và Đề xuất cải tiến thực tế

| Rủi ro phát sinh | Giải pháp đề xuất |
| :--- | :--- |
| **Lỗi tràn bộ nhớ GPU (OOM) khi ảnh quá lớn** | Tự động phát hiện kích thước ảnh đầu vào. Nếu vượt ngưỡng an toàn, kích hoạt chế độ chia nhỏ mảnh ảnh (tile-based processing) và chuyển sang định dạng FP16. Thiết lập cơ chế tự động thử lại (Retry logic) với tài nguyên thấp hơn khi gặp sự cố. |
| **Chi phí GPU vận hành quá cao** | Áp dụng cơ chế Auto Scaling cho các worker AI dựa trên độ dài hàng đợi (queue size). Vào khung giờ thấp điểm, tự động scale số lượng worker GPU về 0 hoặc 1. Cân nhắc sử dụng **AWS Spot Instances** để tiết kiệm đến 70-90% chi phí. |
| **Độ trễ truyền tải file ảnh lớn qua API** | Thay vì bắt client stream file ảnh trực tiếp qua server API, hãy sử dụng cơ chế sinh **S3 Pre-signed URL**. Client sẽ tải ảnh trực tiếp lên S3 và tải ảnh kết quả trực tiếp từ S3 về máy, giúp giải phóng băng thông cho API server. |

## Tổng kết

Kiến trúc này là một sự cân bằng hợp lý giữa chất lượng xử lý hình ảnh và hiệu quả vận hành thực tế trên môi trường cloud. Hệ thống không chỉ giải quyết tốt các bài toán về mở rộng quy mô (scale) mà còn vạch ra lộ trình tối ưu hóa chi phí và hiệu năng rất rõ ràng cho doanh nghiệp.

Các bạn thấy thiết kế này thế nào? Liệu có điểm nào cần tối ưu thêm không? Cùng thảo luận ở bên dưới nhé! Chúc mọi người code vui vẻ!

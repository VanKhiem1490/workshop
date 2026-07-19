---
title: "Blog 3"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Kiến trúc Serverless trên AWS

> **Bài viết gốc:** [AWS Study Group - FCAJ](https://www.facebook.com/photo/?fbid=1545094770504678&set=gm.2206006826830944&idorvanity=660548818043427&locale=vi_VN)

![Kiến trúc Serverless trên AWS](/images/3-BlogsTranslated/Blog3_photo.jpg)

Chào anh em AWS Study Group VN!

Khi bắt đầu dịch chuyển ứng dụng từ mô hình máy chủ truyền thống sang Cloud, "Serverless" luôn là từ khóa được nhắc tới nhiều nhất. Làm thế nào để xây dựng một hệ thống có khả năng tự động co giãn không giới hạn, xử lý hình ảnh bất đồng bộ mượt mà, và đặc biệt là chi phí vận hành bằng 0 khi không có người dùng? 

Hôm nay, mình muốn chia sẻ với anh em một kiến trúc Serverless cực kỳ trực quan và thực tế thông qua hệ thống quản lý sách (Books Service) dưới đây.

## 1. Tại sao lại là Serverless? Nỗi lo vận hành máy chủ biến mất

Thông thường khi triển khai ứng dụng web truyền thống, chúng ta phải đau đầu giải quyết các vấn đề:
* Cần thuê máy chủ (EC2) cỡ nào? Có đủ tải vào giờ cao điểm và có bị lãng phí vào ban đêm không?
* Ai sẽ là người lo việc vá lỗi hệ điều hành, bảo mật và cấu hình cân bằng tải?
* Xử lý các tệp tin hình ảnh lớn tải lên thế nào để tránh gây nghẽn băng thông và treo server?

Kiến trúc Serverless (Không máy chủ) giải quyết tất cả những nỗi lo này. Toàn bộ hạ tầng từ máy chủ, bộ nhớ, đến mạng đều được các dịch vụ của AWS tự động quản lý hoàn toàn. Bạn chỉ cần viết code (Business Logic) và trả tiền chính xác cho mỗi mili-giây mà code thực thi.

## 2. Bóc tách chi tiết luồng xử lý dữ liệu của hệ thống

Hãy cùng đi sâu vào cách mà hệ thống quản lý sách này vận hành khi người dùng tương tác:

### A. Tải giao diện người dùng (Static Web Hosting)
Toàn bộ mã nguồn giao diện (HTML, CSS, JS tĩnh) được đóng gói và lưu trữ trên **Amazon S3 Bucket (host static web)**. Khi người dùng truy cập, S3 sẽ phân phối trực tiếp giao diện này tới trình duyệt. Cách làm này giúp website có tốc độ tải trang cực nhanh mà không cần tốn tiền chạy server Node.js hay Apache 24/7.

### B. Xử lý các tác vụ nghiệp vụ (REST API)
Khi người dùng thực hiện các thao tác trên giao diện, trình duyệt sẽ gửi các API request đến **Amazon API Gateway**. Dịch vụ này đóng vai trò là "cổng bảo vệ và phân tuyến" thông minh, tự động chuyển tiếp yêu cầu đến các hàm **AWS Lambda** tương ứng để xử lý nghiệp vụ:
* **Xem danh sách sách (GET /books):** Yêu cầu được chuyển tới hàm `list_books`. Hàm này thực hiện truy vấn dữ liệu từ bảng **DynamoDB (Books table)** để lấy danh sách sách và trả về cho người dùng.
* **Thêm sách mới (POST /books):** Chuyển tiếp tới hàm `create_book`. Lambda này sẽ ghi nhận thông tin sách mới, đồng thời tải tệp tin ảnh bìa gốc do người dùng tải lên vào **S3 Bucket (store raw file)**.
* **Xóa sách (DELETE /books/:id):** Kích hoạt hàm `delete_book` để xóa thông tin sách trong database, đồng thời dọn dẹp các tệp ảnh bìa liên quan trong **S3 Bucket (store resized file)**.

### C. Xử lý hình ảnh bất đồng bộ (Event-driven Image Resizing)
Một trong những điểm sáng nhất của kiến trúc này là cơ chế **Event-driven (Hướng sự kiện)** để tối ưu hóa hình ảnh:
1. Khi ảnh bìa gốc được tải lên **S3 Bucket (store raw file)**, một sự kiện (S3 Event Trigger) sẽ tự động được phát ra.
2. Sự kiện này kích hoạt hàm **AWS Lambda (resize image)** hoạt động hoàn toàn độc lập và bất đồng bộ.
3. Lambda thực hiện nén ảnh, thay đổi kích thước về tỷ lệ chuẩn của web, sau đó ghi tệp ảnh đã xử lý gọn nhẹ này vào **S3 Bucket (store resized file)**.

## 3. Lợi ích vượt trội của mô hình Serverless này

| Tiêu chí | Ưu thế Serverless |
| :--- | :--- |
| **Tự động co giãn (Scalability)** | Dù có 1 hay 100.000 người truy cập đồng thời, API Gateway và AWS Lambda sẽ tự động scale-up để xử lý mọi yêu cầu và tự động scale-down về 0 khi không có người dùng. |
| **Tối ưu hóa chi phí cực đại** | Không có phí duy trì cố định hàng tháng cho máy chủ. Bạn chỉ trả tiền khi các hàm Lambda thực sự chạy (với mức miễn phí Free Tier cực kỳ rộng rãi lên tới 1 triệu request/tháng). |
| **Độ bền bỉ & Bảo trì cực thấp** | Toàn bộ hệ thống chạy trên các dịch vụ được quản lý hoàn toàn (Fully Managed). AWS chịu trách nhiệm bảo trì phần cứng, hệ điều hành và đảm bảo hệ thống luôn hoạt động ổn định. |

## Kết luận

Mô hình Serverless kết hợp **S3, API Gateway, Lambda và DynamoDB** là sự lựa chọn tối ưu cho các dự án ứng dụng hiện đại. Nó giúp các startup hoặc nhóm phát triển nhỏ dễ dàng triển khai sản phẩm ra thị trường cực kỳ nhanh chóng mà không cần bận tâm về việc quản lý và vận hành hạ tầng phức tạp.

Các bạn có đang áp dụng Serverless cho dự án của mình không? Hãy chia sẻ kinh nghiệm xử lý ảnh bìa hoặc tối ưu DynamoDB của các bạn ở bên dưới nhé! Chúc các bạn code vui vẻ!
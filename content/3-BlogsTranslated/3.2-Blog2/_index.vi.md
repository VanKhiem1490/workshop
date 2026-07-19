---
title: "Blog 2"
date: 2026-05-04
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# Bộ ba dịch vụ AWS trong kiến trúc

> **Bài viết gốc:** [AWS Study Group - FCAJ](https://www.facebook.com/photo?fbid=1541622047518617&set=gm.2202005003897793&idorvanity=660548818043427&locale=vi_VN)

![Amazon VPC, EC2 và Amazon EFS](/images/3-BlogsTranslated/Blog2_photo.jpg)

Trong quá trình thiết kế hạ tầng cho các hệ thống cần chạy nhiều máy chủ web (như WordPress Cluster, hệ thống xử lý tệp tin hoặc các ứng dụng doanh nghiệp), mình thường gặp phải một bài toán kinh điển: **Làm sao để các máy chủ ảo EC2 nằm ở các Availability Zone (AZ) khác nhau có thể đồng thời truy cập, đọc và ghi vào cùng một thư mục dữ liệu chung mà không bị lệch hay mất đồng bộ?**

Thông thường khi xây dựng hệ thống, chúng ta phải giải quyết hai bài toán riêng biệt:
* **Compute (Tính toán):** Các máy chủ EC2 cần co giãn linh hoạt và hoạt động ở nhiều vùng khác nhau để đảm bảo tính sẵn sàng cao (High Availability).
* **Storage (Lưu trữ):** Dữ liệu cần phải được tập trung, đồng nhất và cho phép hàng trăm máy chủ ghi/đọc đồng thời mà không bị xung đột.

Bình thường, chúng ta hay dùng ổ cứng mặc định EBS (Elastic Block Store) cho EC2. Khổ nỗi EBS chỉ gắn được vào một EC2 tại một thời điểm (trừ một số dòng EBS đặc biệt hỗ trợ Multi-Attach nhưng lại bị giới hạn trong cùng một AZ). Nếu muốn chia sẻ file giữa các AZ, ta phải tự dựng cronjob rsync hoặc cấu hình các hệ thống phức tạp như GlusterFS hay NFS thủ công. Làm vậy cực kỳ mệt mỏi và rất dễ lỗi ("cook" dữ liệu lúc nào không hay).

Đó là lý do AWS giới thiệu **Amazon Elastic File System (EFS)** - dịch vụ lưu trữ tệp tin dùng chung (Shared File System) hoạt động ở cấp độ vùng (Region), sử dụng giao thức NFSv4 được quản lý hoàn toàn (fully managed).

## Cơ chế hoạt động của hệ thống qua sơ đồ kiến trúc

Nhìn vào sơ đồ thiết kế phía trên, anh em có thể thấy cách bộ ba dịch vụ này phối hợp cực kỳ nhịp nhàng:

* **Vòng ngoài bảo vệ - Amazon VPC:** Đóng vai trò là mạng riêng ảo giúp cô lập toàn bộ tài nguyên. Hệ thống được chia thành nhiều subnet nằm trên 3 vùng sẵn sàng độc lập: `us-west-2a`, `us-west-2b` và `us-west-2c`.
* **Lớp xử lý - Amazon EC2:** Để đảm bảo ứng dụng không bị sập khi một AZ gặp sự cố, các máy chủ EC2 được phân tán rộng:
  * Tại AZ `us-west-2a`: Có hai máy chủ EC2 hoạt động song song (IP `10.0.1.30` và `10.0.1.31`) thuộc subnet `10.0.1.0/24`.
  * Tại AZ `us-west-2c`: Có một máy chủ EC2 (IP `10.0.4.1`) thuộc subnet `10.0.4.0/24`.
* **Lớp lưu trữ dùng chung - Amazon EFS:** Nằm ở trung tâm và kết nối tới các AZ thông qua một khái niệm gọi là **Mount Target (Điểm gắn kết)**. Mỗi AZ sẽ có một Mount Target với một địa chỉ IP cục bộ riêng:
  * AZ `us-west-2a` sử dụng Mount Target IP `10.0.1.32`.
  * AZ `us-west-2b` sử dụng Mount Target IP `10.0.2.45` (đã cấu hình sẵn subnet `10.0.2.0/24` để sẵn sàng cho việc Auto Scaling máy chủ EC2 về sau).
  * AZ `us-west-2c` sử dụng Mount Target IP `10.0.3.15`.

## Điểm thiết kế mạng cực kỳ thông minh (SecOps)

Điểm mình thấy khá thú vị ở sơ đồ này là cách thiết kế phân tách dải mạng (Subnetting) ở vùng `us-west-2c`. 

Máy chủ EC2 nằm ở subnet `10.0.4.0/24` nhưng Mount Target của EFS lại được đặt ở một subnet riêng biệt là `10.0.3.0/24`. Cách chia này cho phép chúng ta cấu hình Security Groups cực kỳ chặt chẽ: Chỉ cho phép traffic từ subnet chứa EC2 đi vào subnet chứa Mount Target của EFS thông qua cổng NFS truyền thống (Port 2049). Điều này hạn chế tối đa việc các tài nguyên không liên quan trong mạng có thể nhòm ngó hay truy cập trái phép vào kho dữ liệu chung.

## Một số lợi ích nổi bật của mô hình này:

* **Tách biệt hoàn toàn tầng Tính toán và tầng Lưu trữ:** Giúp việc scale-up hoặc scale-down máy chủ EC2 không làm ảnh hưởng tới sự an toàn của dữ liệu. Bạn có thể tắt bớt EC2 hoặc thêm mới mà không cần lo lắng việc di chuyển hay sao chép tệp tin.
* **Không lo hết dung lượng:** Khác với EBS phải cấu hình dung lượng cố định từ đầu, EFS tự động co giãn dung lượng (Elastic) theo lượng file thực tế bạn tải lên. Ghi thêm thì phình ra, xóa đi thì tự thu nhỏ lại, tối ưu hóa chi phí trả theo dung lượng thực tế.
* **Đồng bộ tức thời (Zero rsync):** Mọi thao tác ghi tệp từ máy chủ ở AZ `us-west-2a` sẽ ngay lập tức hiển thị trên máy chủ ở AZ `us-west-2c` mà không cần cấu hình bất kỳ lệnh đồng bộ nào.
* **Tính sẵn sàng cực cao:** Dữ liệu lưu trên EFS được AWS tự động sao lưu và phân tán trên nhiều AZ khác nhau một cách mặc định.

Theo mình, sự kết hợp giữa Amazon VPC, EC2 và EFS đang là bộ khung tiêu chuẩn và đáng tin cậy nhất hiện nay khi bạn cần xây dựng các hệ thống chạy Multi-instance cần chia sẻ tài nguyên tệp tin trên AWS.

**Nguồn tham khảo:**
* **Tài liệu hướng dẫn Amazon EFS:** <https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html>
* **Cách mount EFS vào EC2 instance:** <https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html>
---
title: "Worklog Tuần 10"
date: 2026-05-04
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu tuần 10:
- Xây dựng UI upload ảnh bằng react-dropzone.
- Triển khai cấu hình AI/Standard và scale factor.
- Tạo API client để gọi Backend từ Frontend.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo Upload Area hỗ trợ kéo thả/chọn file, empty state và file selected state.<br>- Hiển thị tên file, dung lượng và preview ảnh. | 06/07/2026 | 06/07/2026 |  |
| 3 | Bổ sung validate định dạng ảnh, giới hạn dung lượng và thông báo lỗi thân thiện.<br>- Thêm chức năng remove/reselect file. | 07/07/2026 | 07/07/2026 |  |
| 4 | Thiết kế Configuration Components cho mode AI/Standard và scale factor.<br>- Disable cấu hình hoặc submit khi chưa có file hợp lệ. | 08/07/2026 | 08/07/2026 |  |
| 5 | Tạo lib/api.ts, đọc NEXT_PUBLIC_API_URL từ .env.local.<br>- Gọi thử GET /health/ready và GET /health/config. | 09/07/2026 | 09/07/2026 |  |
| 6 | Chuẩn hóa TypeScript types cho request/response/error.<br>- Tổng hợp lỗi kết nối và chuẩn bị tích hợp POST upscale. | 10/07/2026 | 10/07/2026 |  |

## Kết quả đạt được tuần 10:
- Hoàn thiện upload flow cơ bản với preview và validate đầu vào.
- Người dùng chọn được mode xử lý và tham số upscale.
- API client đã sẵn sàng để tích hợp endpoint xử lý ảnh.

## Kiến thức / kinh nghiệm học được:
- Biết áp dụng kiến thức nền tảng cloud và tư duy triển khai vào dự án FE/BE thực tế.
- Có thêm kinh nghiệm với Next.js, TypeScript, Tailwind CSS, API client, upload file và xử lý trạng thái bất đồng bộ.
- Rèn luyện kỹ năng tích hợp Frontend - Backend, kiểm thử, debug và viết tài liệu bàn giao.

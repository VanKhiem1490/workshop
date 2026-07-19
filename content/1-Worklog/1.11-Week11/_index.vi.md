---
title: "Worklog Tuần 11"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu tuần 11:
- Tích hợp FE với endpoint xử lý ảnh của Backend.
- Hiển thị kết quả upscale, hỗ trợ download và reset workflow.
- Tối ưu trải nghiệm người dùng khi xử lý ảnh.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo FormData gồm file ảnh, mode và scale factor.<br>- Tích hợp POST /upscale/standard. | 13/07/2026 | 13/07/2026 |  |
| 3 | Tích hợp POST /upscale/ai.<br>- Xử lý response thành công, lỗi server, lỗi endpoint và timeout. | 14/07/2026 | 14/07/2026 |  |
| 4 | Bổ sung trạng thái loading/pending khi Backend xử lý ảnh.<br>- Disable nút submit để tránh gửi request nhiều lần. | 15/07/2026 | 15/07/2026 |  |
| 5 | Xây dựng Result Display: ảnh gốc, ảnh kết quả, thông tin mode/scale và nút download.<br>- Thêm reset/retry để xử lý lại ảnh. | 16/07/2026 | 16/07/2026 |  |
| 6 | Tối ưu Toast/Alert cho success/error, kiểm tra responsive desktop/mobile.<br>- Tổng hợp bug và danh sách cần sửa trước tuần cuối. | 17/07/2026 | 17/07/2026 |  |

### Kết quả đạt được tuần 11:
- Luồng upload - gọi API - nhận kết quả - download hoạt động ở mức cơ bản.
- Nắm được cách debug tích hợp qua Network tab, status code, response schema và log Backend.
- Result Display và error feedback rõ ràng hơn.

### Kiến thức / kinh nghiệm học được:
- Biết áp dụng kiến thức nền tảng cloud và tư duy triển khai vào dự án FE/BE thực tế.
- Có thêm kinh nghiệm với Next.js, TypeScript, Tailwind CSS, API client, upload file và xử lý trạng thái bất đồng bộ.
- Rèn luyện kỹ năng tích hợp Frontend - Backend, kiểm thử, debug và viết tài liệu bàn giao.
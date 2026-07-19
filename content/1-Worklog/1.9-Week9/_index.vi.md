---
title: "Worklog Tuần 9"
date: 2026-05-04
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu tuần 9:
- Chuyển trọng tâm từ học AWS sang áp dụng vào dự án AI Image Upscaling Service.
- Khảo sát FE Next.js và BE API, thống nhất API contract.
- Lập kế hoạch tích hợp upload ảnh, cấu hình upscale và hiển thị kết quả.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Đọc README Frontend, nắm stack Next.js 14 App Router, TypeScript, Tailwind CSS, react-dropzone, lucide-react.<br>- Khảo sát cấu trúc src/app, components, hooks, context, lib và types. | 29/06/2026 | 29/06/2026 | <https://github.com/vuong20031591-hub/upscale-FE> |
| 3 |  Xác định các nhóm component cần triển khai/hoàn thiện: upload, config, result, feedback và ui.<br>- Ghi chú design system: primary violet, CTA cyan, success/error/warning. | 30/06/2026 | 30/06/2026 | <https://github.com/vuong20031591-hub/upscale-BE> |
| 4 | Rà soát API integration: GET /health/ready, GET /health/config, POST /upscale/ai, POST /upscale/standard.<br>- Thiết kế request/response và error schema. | 01/07/2026 | 01/07/2026 |  |
| 5 | Lập luồng người dùng: chọn ảnh, preview, chọn mode, gửi xử lý, xem kết quả, tải ảnh.<br>- Viết testcase cấp cao cho upload/config/result/error handling. | 02/07/2026 | 02/07/2026 |  |
| 6 |  Tổng hợp API contract và checklist tích hợp FE/BE.<br>- Chuẩn bị kế hoạch triển khai UI upload và cấu hình xử lý. | 03/07/2026 | 03/07/2026 |  |

### Kết quả đạt được tuần 9:
- Nắm được kiến trúc Frontend của dự án AI Image Upscaling Service.
- Xác định rõ các endpoint cần tích hợp và luồng người dùng chính.
- Hoàn thành API contract draft và testcase cấp cao cho giai đoạn triển khai.

### Kiến thức / kinh nghiệm học được:
- Biết áp dụng kiến thức nền tảng cloud và tư duy triển khai vào dự án FE/BE thực tế.
- Có thêm kinh nghiệm với Next.js, TypeScript, Tailwind CSS, API client, upload file và xử lý trạng thái bất đồng bộ.
- Rèn luyện kỹ năng tích hợp Frontend - Backend, kiểm thử, debug và viết tài liệu bàn giao.

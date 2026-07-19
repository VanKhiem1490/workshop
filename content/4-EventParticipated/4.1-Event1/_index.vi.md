---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---
# BÀI THU HOẠCH: TỰ ĐỘNG HÓA QUY TRÌNH LÀM VIỆC VỚI TRỢ LÝ AI AMAZON Q VÀ GIAO THỨC MCP

### I. Tổng quan Sự kiện

* **Tên chủ đề:** Tự động hóa quy trình làm việc với trợ lý AI Amazon Q và giao thức MCP
* **Sự kiện:** FCAJ Community Day
* **Thời gian:** 09:00, ngày 23/05/2026
* **Địa điểm:** Tầng 26, Bitexco Financial Tower, Quận 1, TP. Hồ Chí Minh
* **Diễn giả:** Anh Hải An – Cloud Consultant tại C Pacific Việt Nam
* **Vai trò tham gia:** Người tham dự (Attendee)

---

### II. Mục tiêu & Hướng tiếp cận

Buổi chia sẻ tập trung vào các định hướng chiến lược sau:
* **Lan tỏa giải pháp AI:** Giới thiệu hệ sinh thái trợ lý ảo **Amazon Q** do AWS phát triển, hướng đến đối tượng người dùng cuối nhằm đơn giản hóa thao tác làm việc hàng ngày.
* **Tự động hóa toàn diện:** Đề xuất phương án tối ưu hóa hiệu suất thông qua tự động báo cáo, phân tích dữ liệu và tinh chỉnh luồng vận hành doanh nghiệp.
* **Bóc tách công nghệ Agent:** Đi sâu vào bản chất kiến trúc Multi-Agent và vai trò của giao thức Model Context Protocol (MCP) trong việc mở rộng tính năng của các mô hình ngôn ngữ lớn (LLM).
* **Định hình tư duy thiết kế:** Khuyến khích cộng đồng phát triển phần mềm tập trung vào việc giải quyết trực tiếp nhu cầu thực tế của người dùng (Product-driven mindset).

---

### III. Nội dung Kỹ thuật nổi bật

Trong suốt sự kiện, diễn giả đã làm rõ các khía cạnh kỹ thuật cốt lõi:
1. **Triết lý Thiết kế lấy người dùng làm trung tâm (User-centric):** Mọi công cụ công nghệ chỉ thực sự có giá trị khi giải quyết được các điểm nghẽn (pain points) thực tế. Việc áp dụng AI vào xử lý dữ liệu thô và xuất báo cáo tự động giúp tiết kiệm đáng kể thời gian cho các nhà quản lý.
2. **Khả năng Tương thích và Tích hợp:** Amazon Q hỗ trợ kết nối sâu với các ứng dụng văn phòng phổ biến như Microsoft Office 365 (Word, Teams, Outlook) và Google Workspace (Gmail, Calendar), giúp tạo ra trải nghiệm làm việc liền mạch.
3. **Cơ chế hoạt động của Agent và MCP:** AI đơn thuần (LLMs) rất thông minh nhưng thiếu khả năng tương tác trực tiếp với thế giới vật lý hoặc hệ thống nội bộ. Giao thức **MCP đóng vai trò như một chuẩn kết nối chung**, cho phép AI gửi yêu cầu và truy xuất dữ liệu an toàn từ các nền tảng bên thứ ba (Jira, Confluence, Slack) qua các Action API cụ thể.
4. **Trải nghiệm Demo thực tế:**
   * **Phân tích dữ liệu không code:** Tải trực tiếp file Excel thô lên để Amazon Q phân tích và vẽ biểu đồ xu hướng tự động.
   * **Số hóa biên bản cuộc họp:** AI tự ghi âm, chuyển giọng nói thành văn bản, tóm tắt các điểm chính và tự động tạo/gửi email giao việc cho từng thành viên thông qua kết nối MCP.
5. **Mô hình Bảo mật:** Áp dụng chặt chẽ mô hình Trách nhiệm chia sẻ (Shared Responsibility Model), đảm bảo an toàn từ tầng hạ tầng cloud của AWS đến tầng kiểm soát truy cập dữ liệu của khách hàng.

---

### IV. Kiến thức tiếp thu & Ứng dụng thực tiễn

Thông qua nội dung chia sẻ, tôi đã đúc kết được các bài học giá trị:
* **Về tư duy phát triển:** Thiết kế ứng dụng AI cần dịch chuyển từ mô hình hỏi-đáp (chatbot) đơn giản sang mô hình tác vụ tự hành (autonomous agents) để mang lại hiệu quả thực tế cao hơn.
* **Về kiến thức kỹ thuật:** Khắc sâu mô hình: `Agent = Mô hình LLM + Giao thức MCP (thực thi API ngoại vi)`. Hiểu rõ cấu trúc thiết lập để phát triển các luồng làm việc tự động hóa khép kín.
* **Kế hoạch ứng dụng thực tế:** 
  * Sử dụng Amazon Q để tăng tốc độ phân tích báo cáo công việc cá nhân hàng tuần.
  * Nghiên cứu viết các MCP Server tùy chỉnh để tích hợp các LLM với hệ thống quản lý công việc đang sử dụng, giúp tự động tạo tác vụ sau các buổi họp nhóm.

---

### V. Trải nghiệm thực tế & Đánh giá

* **Chia sẻ từ diễn giả:** Anh Hải An đã truyền tải kiến thức một cách cuốn hút và thực tế, giúp người nghe hiểu rõ cách tiếp cận và giải quyết vấn đề kỹ thuật dựa trên nhu cầu của khách hàng.
* **Trải nghiệm kỹ thuật:** Việc trực tiếp quan sát cách prompt nội bộ dịch yêu cầu tự nhiên thành lệnh hành động cho AI mang lại góc nhìn trực quan sâu sắc về cơ chế vận hành của MCP.
* **Định hướng công nghệ mới:** Sự kiện mở ra khả năng tự xây dựng và tùy biến các MCP Server riêng biệt để phục vụ tối đa nhu cầu đặc thù của từng dự án cụ thể mà tôi tham gia.

> **Tổng kết chung:** Sự kiện mang lại giá trị lớn cả về mặt kỹ thuật đám mây lẫn tư duy thiết kế sản phẩm, giúp tôi định hình rõ ràng phương pháp xây dựng các giải pháp tự động hóa thông minh trong tương lai.

![Hình ảnh tham gia sự kiện](/images/4-EventParticipated/tang26.jpg)

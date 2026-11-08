# BÀI LÀM: ĐẠI TU HỆ THỐNG KIOSK SẠC ĐIỆN TOÀN THÀNH PHỐ

* **Môn học:** Nhập Môn Công Nghệ Thông Tin (IT108)  
* **Tác giả:** Lê Hồ Thanh Thảo (MSV: N26DTCN030)  
* **Vị trí giả lập:** Kỹ thuật viên trưởng ca trực tại Trung tâm Điều hành Mạng lưới (NOC) — VoltCharge  

---

## PHẦN 1: TRA CỨU CẤP TỐC TÀI LIỆU & HOTFIX (LESSON 01 & 02)

### 1. Câu lệnh Google Search chuẩn xác tìm tệp DLL chính thức

Để tìm chính xác liên kết tải tệp driver `CanBus_Interface.dll` chính thức từ nhà sản xuất phần cứng [kvaser.com](https://kvaser.com), loại trừ hoàn toàn các trang web quảng cáo, phần mềm crack hoặc trang chia sẻ file không an toàn:

```text
site:kvaser.com "CanBus_Interface.dll" -crack -dll-files -download-site -ad
```
### 2. Quy trình tìm Workaround trên GitHub Issues
Khi hệ thống gặp sự cố thiếu tệp DLL khẩn cấp, kỹ thuật viên truy cập tab Issues trên repository mã nguồn mở của hệ thống Kiosk trạm sạc và thực hiện 4 bước lọc nhanh sau:

Sử dụng thanh tìm kiếm bộ lọc (Search Filter):
Nhập từ khóa lỗi kết hợp điều kiện lọc trạng thái:is:issue is:open "CanBus_Interface.dll" label:workaround (hoặc label:hotfix).

Ưu tiên sắp xếp bài viết:
Sắp xếp danh sách kết quả theo Most commented (Nhiều thảo luận nhất) hoặc Thumbs up reactions (👍) để xác định các giải pháp được cộng đồng kỹ thuật viên xác nhận có hiệu quả cao nhất.

Đọc kỹ các bài thảo luận có nhãn hoặc được ghim (Pinned/Approved Workaround):
Tìm kiếm các đoạn mã cấu hình (config flags) cho phép vô hiệu hóa kiểm tra driver khi khởi động.

Ví dụ: Sửa tệp cấu hình trạm sạc config.json chuyển "CHECK_CANBUS_DRIVER": true thành false hoặc khởi chạy ứng dụng với cờ --bypass-canbus-check.

Áp dụng tạm thời:
Hướng dẫn KTV tại trạm triển khai lệnh bypass này để mở lại dịch vụ sạc cho tài xế ngay lập tức trong khi chờ đội ngũ R&D phát hành bản vá chính thức.

## PHẦN 2: ỨNG DỤNG TRỢ LÝ AI GIẢI MÃ & XỬ LÝ XUNG ĐỘT (LESSON 03)
### 3. Prompt 3 Thành Phần giải mã lỗi EACCES gửi Trợ lý AI
Cấu trúc Prompt 3 thành phần (Bối cảnh — Lỗi chi tiết — Yêu cầu đầu ra) gửi Trợ lý AI (ChatGPT / Gemini):
[BỐI CẢNH]
Tôi là kỹ thuật viên NOC đang gỡ lỗi khẩn cấp trên máy tính Kiosk chạy Windows 10/11 tại trạm sạc xe điện VoltCharge. Hệ thống vừa tự động cập nhật lên phiên bản v2.4.2 nhưng dịch vụ không thể ghi dữ liệu lịch sử sạc.

[THÔNG BÁO LỖI CHI TIẾT]
Cửa sổ dòng lệnh Node.js/System báo lỗi dồn dập:
EACCES: permission denied, open 'C:\VoltLogs\charge.log'

[YÊU CẦU ĐẦU RA]
1. Giải thích ngắn gọn nguyên nhân gốc rễ (Root Cause) gây ra lỗi EACCES trong trường hợp này.
2. Cung cấp hướng dẫn chính xác 3 bước xử lý nhanh bằng dòng lệnh Terminal (PowerShell/CMD với quyền Administrator) hoặc GUI để cấp quyền ghi tệp cho dịch vụ mà không cần khởi động lại Kiosk.


### 4. Quy trình 2 dòng lệnh Terminal giải phóng cổng 8080 bị chiếm dụng
Khắc phục lỗi Port 8080 already in use trên máy tính trạm Kiosk (Windows) mà không cần khởi động lại hệ thống bằng 2 bước trên PowerShell / Command Prompt (Run as Administrator):

Bước 1: Tra cứu Process ID (PID) đang chiếm dụng cổng 8080
netstat -ano | findstr :8080

Bước 2: Dừng cưỡng chế tiến trình gây nghẽn bằng taskkill
taskkill /F /PID 4512


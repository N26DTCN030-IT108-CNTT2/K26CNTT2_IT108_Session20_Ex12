PHẦN 3: CHUẨN HÓA BUG REPORT KHẨN CẤP GỬI ĐỘI NGŨ R&D (LESSON 04)
5. Thông báo Báo cáo Lỗi Khẩn Cấp (Emergency Bug Report)
To: CTO, Head of Software Engineering, Backend R&D Team

From: Lê Hồ Thanh Thảo — Lead NOC Engineer

Date: Friday, Peak Hour (17:30 PM)

Subject: [EMERGENCY BUG REPORT][CRITICAL] Auto-update v2.4.2 Cause System-Wide Outage at 20 Charging Stations — Request Immediate Rollback

1. Context (Bối cảnh sự cố)
Thời gian xảy ra: 17:30 chiều thứ Sáu (Giờ cao điểm mưa dông lớn).

Phạm vi ảnh hưởng: Toàn bộ 20 trạm sạc VoltCharge toàn thành phố.

Tác động kinh doanh: Hàng trăm xe taxi điện không thể sạc, gây ùn tắc và nguy cơ khủng hoảng truyền thông.

Tác nhân kích hoạt: Hệ thống tự động đẩy bản cập nhật Auto-update v2.4.2 xuống các Kiosk lúc 17:25 PM.

2. Expected Behavior (Hành vi mong muốn)
Bản cập nhật v2.4.2 nâng cấp mượt mà, dịch vụ Kiosk khởi động bình thường, tự động tải đầy đủ driver kết nối phần cứng CanBus, duy trì quyền ghi log vào thư mục C:\VoltLogs\ và lắng nghe cổng mạng 8080 để nhận lệnh điều phối sạc.

3. Actual Behavior (Hành vi thực tế xảy ra)
Ngay sau khi nâng cấp lên v2.4.2, toàn bộ 20 trạm sạc bị tê liệt với 3 sự cố dây chuyền:

Màn hình Kiosk bật cửa sổ lỗi màu đỏ, thiếu file driver DLL nên không nhận diện được trụ sạc.

Dịch vụ báo lỗi mất quyền truy cập tệp ghi log charge.log.

Khi cố khởi động lại dịch vụ điều phối, cổng mạng 8080 bị treo và báo đã bị chiếm dụng.

4. Logs & Error Details (Nhật ký lỗi chi tiết)
-- Log 1: Driver Missing Error --
[CRITICAL ERROR] Failed to load driver 'CanBus_Interface.dll'. 
Error 126: The specified module could not be found. System.IO.FileNotFoundException at VoltCharge.Core.CanBusManager.Init()

-- Log 2: File Permission Error --
[ERROR] EACCES: permission denied, open 'C:\VoltLogs\charge.log'
    at FileHandle.open (node:internal/fs/promises:525:11)
    at async LogWriter.write (C:\VoltService\app.js:42:5)

-- Log 3: Port Binding Collision --
[FATAL] EADDRINUSE: address already in use :::8080
    at Server.setupListenHandle [as _listen2] (node:net:1463:16)
    at listenInCluster (node:net:1511:12)

YÊU CẦU HÀNH ĐỘNG KHẨN CẤP (ACTION REQUIRED):
DỪNG TÍNH NĂNG: Lập tức ngắt tính năng Auto-update v2.4.2 trên toàn bộ hệ thống server điều phối.

ROLLBACK: Phát hành lệnh Rollback tự động đưa toàn bộ 20 trạm Kiosk về phiên bản ổn định v2.4.1.

HOTFIX: Kiểm tra lại quy trình đóng gói installer v2.4.2 (thiếu tệp DLL trong bộ cài và phân quyền sai thư mục C:\VoltLogs).
























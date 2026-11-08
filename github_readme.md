# ⚡ VoltCharge Kiosk - AI-Assisted System Troubleshooting & Security Guide

![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)
![Security](https://img.shields.io/badge/Security-Sanitized-orange.svg)
![OS](https://img.shields.io/badge/OS-Windows%2011-0078D4.svg)

Hướng dẫn chuẩn hóa quy trình xử lý sự cố kỹ thuật bằng Trợ lý AI (ChatGPT / Gemini) kết hợp thực thi bảo mật an toàn thông tin dành cho Kỹ thuật viên Vận hành Hệ thống trạm sạc thông minh **VoltCharge**.

---

## 📌 Mục mục nội dung (Table of Contents)
- [1. Bối cảnh Sự cố (Incident Context)](#1-bối-cảnh-sự-cố-incident-context)
- [2. Phân tích Hiện tượng Ảo giác AI (AI Hallucination)](#2-phân-tích-hiện-tượng-ảo-giác-ai-ai-hallucination)
- [3. Khung Prompt 3 Thành phần (3-Component Prompt Framework)](#3-khung-prompt-3-thành-phần-3-component-prompt-framework)
- [4. Chẩn đoán & Kịch bản Khắc phục Sự cố](#4-chẩn-đoán--kịch-bản-khắc-phục-sự-cố)
- [5. Quy trình Làm sạch Dữ liệu Bí mật (Sanitization Protocol)](#5-quy-trình-làm-sạch-dữ-liệu-bí-mật-sanitization-protocol)
- [6. 3 Nguyên tắc Vàng An toàn Thông tin](#6-3-nguyên-tắc-vàng-an-toàn-thông-tin)

---

## 1. Bối cảnh Sự cố (Incident Context)

Vào ca trực đêm tại Bến xe Miền Đông, **Trụ sạc nhanh số #04** của Kiosk VoltCharge đột ngột dừng ngắt nguồn cấp điện cho xe taxi. Tiến trình nền `ControllerDaemon` sập và xuất hiện nhật ký lỗi thô (Raw Error Log) trên Terminal:

```text
[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
   at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
   at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.
```

---

## 2. Phân tích Hiện tượng Ảo giác AI (AI Hallucination)

Khi gặp sự cố, việc đặt câu hỏi mù mờ như:  
❌ *"AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?"*

Sẽ dẫn đến **Hiện tượng Ảo giác AI (AI Hallucination)** vì các lý do:
1. **Thiếu Ngữ cảnh Kỹ thuật (Context):** AI không xác định được hệ điều hành (Windows 11), ứng dụng hay kiến trúc phần mềm.
2. **Thiếu Mã lỗi Nguyên văn (Raw Error Log):** Không có chuỗi ngoại lệ `UnauthorizedAccessException` và vết thực thi (*Stack Trace*), AI không thể phân tích vấn đề phân quyền.
3. **Cơ chế Dự đoán từ (Next-token Prediction):** AI tự "điền vào chỗ trống" bằng các dữ liệu phổ biến về phần cứng trên mạng (khuyên tháo vỏ trạm sạc, kiểm tra dây điện), gây sai lệch hoàn toàn bản chất phần mềm và đe dọa an toàn thiết bị.

---

## 3. Khung Prompt 3 Thành phần (3-Component Prompt Framework)

Để nhận phản hồi chính xác 100%, kỹ thuật viên bắt buộc sử dụng cấu trúc Prompt chuẩn kỹ sư:

```text
[CONTEXT]
Tôi đang vận hành hệ thống Kiosk nạp năng lượng VoltCharge (trạm sạc số #04) chạy trên hệ điều hành Windows 11 tại Bến xe Miền Đông. Trụ sạc đột ngột dừng hoạt động và tiến trình ControllerDaemon bị sập khi khởi động.

[RAW ERROR LOG]
[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
   at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
   at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.

[INSTRUCTION]
1. Dịch và giải thích nguyên nhân gốc rễ của lỗi trên bằng tiếng Việt đơn giản, dễ hiểu cho kỹ thuật viên mới.
2. Đưa ra 3 bước xử lý cụ thể tại chỗ trên Windows 11 để khắc phục lỗi phân quyền ghi file, giúp trạm sạc hoạt động trở lại.
```

---

## 4. Chẩn đoán & Kịch bản Khắc phục Sự cố

### 🔍 Nguyên nhân gốc rễ (Root Cause)
Phần mềm cần ghi nhật ký phiên sạc vào tệp `C:\Program Files\VoltCharge\data\session.log`. Thư mục `C:\Program Files\` thuộc khu vực bảo vệ nghiêm ngặt của hệ điều hành Windows. Tiến trình đang chạy dưới quyền tài khoản người dùng thông thường (`Standard User`), do đó bị Windows chặn quyền ghi tệp (`Access is denied`), dẫn đến sập ứng dụng (Exit code 5).

### 🛠️ Kịch bản 3 Bước Xử lý tại chỗ trên Windows 11

| Bước | Hành động | Chi tiết thao tác |
| :--- | :--- | :--- |
| **Bước 1** | **Chạy bằng quyền Quản trị viên** | Chuột phải vào `ControllerDaemon.exe` $\rightarrow$ Chọn **Run as administrator**.<br>*(Lâu dài: Properties $\rightarrow$ Compatibility $\rightarrow$ Tích chọn Run this program as an administrator $\rightarrow$ Apply).* |
| **Bước 2** | **Cấp quyền ghi (NTFS Permissions)** | Mở thư mục `C:\Program Files\VoltCharge\` $\rightarrow$ Chuột phải thư mục `data` $\rightarrow$ **Properties** $\rightarrow$ Thẻ **Security** $\rightarrow$ **Edit...** $\rightarrow$ Chọn **Users** $\rightarrow$ Tích **Full control** / **Write** $\rightarrow$ **Apply**. |
| **Bước 3** | **Tái khởi động & Kiểm tra** | Khởi chạy lại dịch vụ VoltCharge $\rightarrow$ Kiểm tra tệp `session.log` đã cập nhật mốc thời gian mới $\rightarrow$ Kiểm tra trụ sạc #04 cấp điện trở lại cho xe. |

---

## 5. Quy trình Làm sạch Dữ liệu Bí mật (Sanitization Protocol)

Trường hợp đoạn mã lỗi có chứa **Secret Keys / Access Tokens**:

```text
# TRƯỚC KHI SANITIZE (NGUY HIỂM ❌):
2026-09-21 21:15:30 [ERROR] Connection failed. API_KEY: volt_secret_token_889922

# SAU KHI SANITIZE (AN TOÀN ✅):
2026-09-21 21:15:30 [ERROR] Connection failed. API_KEY: [REDACTED_SECRET_KEY]
```

> ⚠️ **Cảnh báo:** Không bao giờ gửi mã khóa bí mật, Token hay thông tin mật khẩu thật lên hệ thống Chatbot AI công cộng nhằm ngăn chặn nguy cơ rò rỉ an ninh thông tin và các cuộc tấn công chiếm quyền điều khiển từ xa.

---

## 6. 3 Nguyên tắc Vàng An toàn Thông tin

1. 🚫 **Không copy-paste mù quáng:** Luôn kiểm tra kỹ các câu lệnh hệ thống (đặc biệt là lệnh PowerShell/Bash chạy bằng quyền Admin) trước khi thực thi.
2. 🎯 **Chống Ảo giác AI:** Luôn cung cấp đầy đủ Ngữ cảnh và Log lỗi thô. Thêm ràng buộc: *"Nếu không chắc chắn, hãy trả lời không biết, tuyệt đối không tự đoán mò."*
3. 🔐 **Bảo vệ Secrets & PII:** Tuyệt đối lọc bỏ API Keys, Mật khẩu, SSH Keys và Thông tin cá nhân khách hàng (PII) trước khi dán vào khung chat AI.

---

© 2026 **VoltCharge Engineering Team**. Released under the [MIT License](LICENSE).
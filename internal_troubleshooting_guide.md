# 🛠️ Quy Trình Troubleshooting & Xử Lý Lỗi Nội Bộ (Internal Troubleshooting Protocol)

Tài liệu này quy định quy trình xử lý lỗi môi trường (Docker, Port Conflict, Dependency,...) nhằm tối ưu thời gian làm việc, tăng tính chủ động cho thành viên và giữ kênh giao tiếp nội bộ luôn gọn gàng.

---

## 📋 1. Mẫu Bug Report Template (Bắt Bắt Buộc)

> **Quy tắc:** Mọi câu hỏi nhờ hỗ trợ kỹ thuật gửi vào kênh chat nội bộ (Slack/Discord/Zalo) **bắt buộc** phải điền đầy đủ theo cấu trúc 4 mảnh ghép dưới đây.

```markdown
### 🐛 BUG REPORT

**1. Context (Ngữ cảnh)**
- Hệ điều hành / Môi trường: [VD: Windows 11 (WSL2) / Docker Desktop v4.28 / Node.js v18]
- Lệnh / Thao tác vừa thực hiện: [VD: Chạy `docker-compose up` để khởi chạy backend]

**2. Expected Behavior (Kỳ vọng)**
- [VD: Container `app-backend` khởi động thành công và lắng nghe tại port 8080]

**3. Actual Behavior (Thực tế)**
- [VD: Container báo lỗi gán cổng và dừng đột ngột (Exit Code 1)]

**4. Logs & Các bước đã tự xử lý (Nhật ký lỗi & Thử nghiệm)**
- Log lỗi / Terminal output:
  ```text
  Error response from daemon: driver failed programming external connectivity on endpoint app-backend: Bind for 0.0.0.0:8080 failed: port is already allocated
  ```
- Kết quả từ Quy trình 3 bước đã thử:
  - [x] Step 1 (Google): Đã tìm thấy các bài viết về Port 8080 conflict.
  - [x] Step 2 (Community/KB): Kiểm tra `netstat` phát hiện PID 4120 đang chiếm port.
  - [x] Step 3 (AI Diagnosis): Đã thử lệnh `taskkill` nhưng báo từ chối truy cập (Access Denied).
```

---

## 🔄 2. Quy Trình 3 Bước Tra Cứu Tự Xử Lý (Self-Healing Workflow)

Trước khi gửi tin nhắn nhờ hỗ trợ, các thành viên cần thực hiện lần lượt 3 bước tra cứu để tự giải quyết **80% vấn đề**:

```
[ Phát hiện lỗi ]
        │
        ▼
┌─────────────────┐      Thành công
│  1. Googling    ├──────────────────► [ Khắc phục & Chuyển sang công việc ]
└───────┬─────────┘
        │ Chưa xong
        ▼
┌─────────────────┐      Thành công
│ 2. Community    ├──────────────────► [ Khắc phục & Cập nhật Knowledge Base ]
└───────┬─────────┘
        │ Chưa xong
        ▼
┌─────────────────┐      Thành công
│3. AI Translation├──────────────────► [ Khắc phục & Làm việc tiếp ]
└───────┬─────────┘
        │ Bế tắc hoàn toàn
        ▼
[ Gửi Bug Report vào Kênh Chat Nội Bộ ]
```

---

### 🔍 Bước 1: Googling (Lọc Nhiễu & Tìm Thông Tin Gốc)
* **Mục tiêu:** Tìm kiếm giải pháp nguyên bản từ các trang chuyên ngành bằng từ khóa chuẩn.
* **Cách thực hiện:**
  1. Loại bỏ các thông tin cá nhân/đường dẫn nội bộ khỏi thông báo lỗi (ví dụ: `C:\Users\Admin\...`).
  2. Bọc chuỗi báo lỗi cố định trong dấu ngoặc kép `""` để tìm chính xác.
  3. **Cú pháp mẫu:** `"Bind for 0.0.0.0:8080 failed: port is already allocated" "docker"`

---

### 🌐 Bước 2: Community & Internal KB Search (Cộng Đồng & Bộ Nhớ Nội Bộ)
* **Mục tiêu:** Tận dụng giải pháp đã được xác nhận bởi cộng đồng hoặc đồng nghiệp trong nhóm.
* **Cách thực hiện:**
  1. **Tra cứu cộng đồng:** Đọc các chủ đề có đánh dấu `[Accepted]` hoặc lượt vote cao trên **Stack Overflow**, **GitHub Issues**, **Reddit** (`r/docker`, `r/devops`).
  2. **Tra cứu nội bộ:** Tìm kiếm từ khóa chính trên trang Notion/Wiki hoặc lịch sử chat của nhóm để xem lỗi này đã từng xảy ra chưa.

---

### 🤖 Bước 3: AI Translation & Diagnosis (Chẩn Đoán Bằng AI)
* **Mục tiêu:** Dùng AI (ChatGPT/Claude/Gemini) đóng vai trò Senior Developer để giải thích nguyên nhân gốc rễ và hướng dẫn sửa lỗi từng bước.
* **Prompt Mẫu Bắt Buộc Sử Dụng:**

> *"Tôi đang gặp lỗi sau khi chạy ứng dụng:*
> *- Môi trường: [Điền OS, phiên bản công cụ]*
> *- Lệnh vừa chạy: [Điền câu lệnh]*
> *- Log lỗi:*
> ```text
> [Dán đoạn log lỗi vào đây]
> ```
> *Hãy:*
> 1. Giải thích nguyên nhân gốc rễ (Root cause) của lỗi này bằng tiếng Việt dễ hiểu.
> 2. Đưa ra các bước lệnh Terminal chính xác để tôi khắc phục vấn đề trên hệ điều hành của mình."*

---

## ⚡ 3. Quy Tắc Vận Hành Vàng (Operational Rules)

1. **No Template = No Answer:** Tin nhắn hỏi lỗi không theo mẫu **Bug Report Template** sẽ bị từ chối tiếp nhận cho đến khi điền đủ thông tin.
2. **Cập nhật Kiến thức (Log to KB):** Khi một lỗi mới được giải quyết xong nhờ sự hỗ trợ của team, người gặp lỗi có trách nhiệm dành **2 phút** lưu lại nguyên nhân + giải pháp vào trang Knowledge Base chung.
3. **Tôn trọng thời gian:** Đảm bảo đã đi qua 3 bước tra cứu tự động trước khi tag (mention) thành viên khác vào hỗ trợ.
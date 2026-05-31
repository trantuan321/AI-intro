# 🩺 Hệ Thống Chuyên Gia Chẩn Đoán Cảm Cúm & Dị Ứng

> Bài Tập Lớn — Môn Nhập Môn Trí Tuệ Nhân Tạo  
> Đại học Bách Khoa Hà Nội

---

## 📋 Mô tả

Hệ thống chuyên gia hỗ trợ chẩn đoán sơ bộ **cảm cúm** và **dị ứng** dựa trên phương pháp **Rule-Based Reasoning** kết hợp thuật toán **Forward Chaining** với cơ chế **Weighted Scoring** động. Người dùng nhập các triệu chứng đang có, hệ thống suy luận và trả về kết quả chẩn đoán cùng giải thích chi tiết các luật đã được kích hoạt.

---

## ⚙️ Yêu cầu hệ thống

| Thành phần | Yêu cầu |
|---|---|
| Trình duyệt | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+ |
| Kết nối internet | Chỉ cần khi tải font chữ lần đầu (tùy chọn) |
| Hệ điều hành | Windows / macOS / Linux |
| Phần mềm cài đặt | **Không yêu cầu** |

---

## 📦 Các gói phần mềm sử dụng

Hệ thống được xây dựng hoàn toàn bằng công nghệ web tiêu chuẩn, **không phụ thuộc bất kỳ thư viện hay framework nào**. Toàn bộ mã nguồn nằm trong một file HTML duy nhất.

| Công nghệ | Phiên bản | Mục đích | Nguồn |
|---|---|---|---|
| HTML5 | Tiêu chuẩn W3C | Cấu trúc giao diện, form nhập liệu | Tích hợp sẵn trong trình duyệt |
| CSS3 | Tiêu chuẩn W3C | Giao diện dark mode, animation, responsive | Tích hợp sẵn trong trình duyệt |
| JavaScript | ES6+ | Toàn bộ logic AI (Knowledge Base, Inference Engine, Weighted Scoring) | Tích hợp sẵn trong trình duyệt |
| Google Fonts | API v2 | Font chữ: *Be Vietnam Pro*, *Playfair Display* | Tải tự động qua CDN (cần internet) |

> **Lưu ý:** Google Fonts chỉ được dùng để hiển thị font chữ. Nếu không có kết nối internet, hệ thống vẫn hoạt động bình thường với font mặc định của trình duyệt.

---

## 🚀 Hướng dẫn cài đặt và chạy chương trình

### Bước 1 — Tải mã nguồn

```
chan-doan-benh.html
```

Đảm bảo file `chan-doan-benh.html` đã được tải về máy.

### Bước 2 — Mở chương trình

**Cách 1 — Mở trực tiếp (khuyến nghị):**

Nhấp đúp vào file `chan-doan-benh.html` → trình duyệt mặc định sẽ tự động mở chương trình.

**Cách 2 — Mở bằng trình duyệt cụ thể:**

```
# Windows
start chrome chan-doan-benh.html

# macOS
open -a "Google Chrome" chan-doan-benh.html

# Linux
google-chrome chan-doan-benh.html
# hoặc
firefox chan-doan-benh.html
```

**Cách 3 — Dùng Live Server (VS Code):**

Nếu có cài Visual Studio Code và extension **Live Server**:
1. Mở thư mục chứa file trong VS Code
2. Chuột phải vào `chan-doan-benh.html` → chọn **"Open with Live Server"**

> ✅ Không cần cài đặt Node.js, Python hay bất kỳ runtime nào.  
> ✅ Không cần chạy lệnh build hay compile.  
> ✅ Không cần kết nối internet (ngoại trừ font chữ).

---

## 📖 Hướng dẫn sử dụng

Sau khi mở chương trình trên trình duyệt, thực hiện theo 3 bước:

**Bước 1 — Chọn triệu chứng**  
Tích chọn tất cả các triệu chứng hiện đang có trong hai nhóm:
- Nhóm triệu chứng cảm cúm (7 triệu chứng)
- Nhóm triệu chứng dị ứng (6 triệu chứng)

**Bước 2 — Chọn mức độ**  
Chọn một trong ba mức độ:
- 🟢 Nhẹ — không ảnh hưởng sinh hoạt
- 🟡 Vừa — hơi khó chịu
- 🔴 Nặng — ảnh hưởng nhiều

**Bước 3 — Chọn thời gian**  
Chọn thời gian xuất hiện triệu chứng:
- Dưới 1 ngày / 1–3 ngày / 4–7 ngày / Hơn 1 tuần / Thường xuyên theo mùa

Nhấn nút **"Phân Tích & Chẩn Đoán"** để xem kết quả.

---

## 📁 Cấu trúc dự án

```
📦 BTL-NhapMonAI/
├── 📄 chan-doan-benh.html     # Toàn bộ mã nguồn hệ thống (HTML + CSS + JS)
├── 📄 README.md               # Tài liệu hướng dẫn (file này)
└── 📄 BaoCao_BTL.pdf          # Báo cáo bài tập lớn
```

---

## 🧠 Kiến trúc hệ thống

```
┌─────────────────────────────────────────┐
│            Giao diện người dùng          │
│         (HTML5 + CSS3 — 3 bước)         │
└────────────────────┬────────────────────┘
                     │ Triệu chứng + Mức độ + Thời gian
                     ▼
┌─────────────────────────────────────────┐
│           Inference Engine              │
│    (Forward Chaining + Weighted Score)  │
└──────────┬──────────────────┬──────────┘
           │                  │
           ▼                  ▼
┌──────────────────┐  ┌──────────────────┐
│  Knowledge Base  │  │  Working Memory  │
│  (11 luật IF–    │  │  (13 triệu chứng │
│   THEN có W)     │  │   + ngữ cảnh)    │
└──────────────────┘  └──────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────┐
│              Kết quả                    │
│  Chẩn đoán + Độ tin cậy + Fired Rules  │
└─────────────────────────────────────────┘
```

---

## ⚠️ Lưu ý quan trọng

> Kết quả của hệ thống **chỉ mang tính tham khảo**, không thay thế chẩn đoán của bác sĩ. Vui lòng tham khảo ý kiến chuyên gia y tế khi triệu chứng nặng hoặc kéo dài.

---

## 👥 Thông tin nhóm

| Họ và tên | MSSV | Vai trò |
|---|---|---|
| Phan Phạm Tuấn Anh | 202416410 | Đưa ra ý tưởng nội dung, thực hiện, viết báo cáo |
| Lê Vĩnh Quốc | 202416595 | Đưa ra ý tưởng thuật toán, thực hiện, thuyết trình |


**Giảng viên hướng dẫn:** PGS.TS. Lê Thanh Hương  
**Học kỳ:** II — Năm học 2025 - 2026

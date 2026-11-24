# Engraver Note App

Ứng dụng ghi chú desktop đa nền tảng được xây dựng bằng Python và [CustomTkinter](https://github.com/TomSchimansky/CustomTkinter), hỗ trợ ghi chú văn bản giàu định dạng, danh sách việc cần làm (checklist), đính kèm đa phương tiện, bảo mật bằng mật khẩu, nhắc nhở, lịch biểu và xuất/nhập nhiều định dạng.

---

## Tính năng chính

- **Nhiều loại ghi chú**:
  - **Text Note**: Soạn thảo văn bản hỗ trợ định dạng (in đậm, in nghiêng, gạch chân, màu chữ), phóng to/thu nhỏ tài liệu (zoom) và chèn tệp media.
  - **Checklist Note**: Danh sách công việc với checkbox đánh dấu hoàn thành.
  - **Media Note**: Quản lý tệp đính kèm ảnh (`.png`, `.jpg`, `.jpeg`, `.gif`, `.bmp`, `.webp`), âm thanh (`.mp3`, `.wav`, `.ogg`, `.m4a`, `.aac`, `.flac`) và video (`.mp4`, `.mov`, `.avi`, `.mkv`, `.webm`, `.m4v`), giới hạn tối đa 100 MB/tệp.
- **Tổ chức & Tìm kiếm**:
  - Phân loại theo **Thư mục** (hỗ trợ thư mục con) và **Thẻ (Tag)**.
  - Tìm kiếm ghi chú trên thanh bên và tìm kiếm trực tiếp trong nội dung ghi chú đang mở.
  - Mục **Bị xóa gần đây** giúp khôi phục ghi chú đã xóa.
- **Hoàn tác / Làm lại (Undo / Redo)**:
  - Áp dụng Command Pattern để quản lý lịch sử thao tác thêm, sửa, xóa ghi chú.
- **Bảo mật ghi chú**:
  - Khóa/mở khóa ghi chú bằng mật khẩu. Mật khẩu được băm bằng `PBKDF2-HMAC-SHA256` kèm salt (120.000 vòng) trước khi lưu vào SQLite, hỗ trợ mã hóa Fernet (`cryptography`).
- **Nhắc nhở, Deadline & Lịch biểu**:
  - Đặt thời gian nhắc nhở và hạn chót (deadline) cho ghi chú.
  - Dịch vụ chạy nền kiểm tra SQLite và gửi thông báo desktop (`plyer`, hỗ trợ Windows và macOS).
  - Cửa sổ **Lịch biểu (Calendar View)** trực quan và hỗ trợ chạy thu nhỏ xuống khay hệ thống (**System Tray** qua `pystray`).
- **Nhập / Xuất dữ liệu linh hoạt**:
  - **Xuất**: Markdown (`.md`), PDF (`.pdf` hỗ trợ font Unicode kèm ảnh đính kèm), gói lưu trữ `.zip` (chứa ghi chú và media), và **ZIP có mật khẩu** (`pyzipper`), cùng tính năng mở/giải nén lại gói ZIP.
  - **Nhập**: Hỗ trợ `.txt`, `.md`, `.docx` (`docx2txt`) và `.pdf` (`PyMuPDF`), tự động tách văn bản và ảnh đính kèm vào ghi chú mới.
- **Tùy biến giao diện & Đa ngôn ngữ**:
  - Chế độ hiển thị: `System`, `Light`, `Dark`.
  - **10 chủ đề màu** trong `config/themes/`: `amber`, `blue`, `dark-blue`, `green`, `pastel-pink`, `rose`, `stone`, `sunflower`, `teal`, `violet`.
  - Hỗ trợ **Tiếng Việt** (`vi`) và **Tiếng Anh** (`en`).

---

## Cấu trúc dự án

```text
EngraverNoteApp/
├── main.py                  # Điểm khởi chạy ứng dụng
├── requirements.txt         # Danh sách thư viện phụ thuộc
├── assets/
│   └── fonts/               # Font Unicode phục vụ xuất PDF (ArialUnicode.ttf)
├── commands/                # Command Pattern cho Undo / Redo
├── config/
│   └── themes/              # Các tệp cấu hình chủ đề màu (.json)
├── data/                    # SQLite DatabaseManager, FileSystemManager, NoteRepository
├── model/                   # Các lớp dữ liệu (TextNote, ChecklistNote, MediaNote, Folder, Tag, NoteFactory)
├── services/                # Nghiệp vụ: export, media, reminder, notification, search, security, settings, theme, i18n
├── tests/                   # Bộ kiểm thử đơn vị (unittest)
└── ui/                      # Giao diện CustomTkinter (MainWindow, Sidebar, Editor, CalendarView, DatePicker, ColorPicker)
```

---

## Cài đặt & Chạy ứng dụng

### Yêu cầu hệ thống
- Python 3.10+ (khuyến nghị)

### 1. Cài đặt thư viện phụ thuộc

```bash
pip install -r requirements.txt
```

### 2. Chạy ứng dụng

```bash
python main.py
```

---

## Phím tắt trong trình soạn thảo

| Phím tắt | Chức năng |
| :--- | :--- |
| `Ctrl + B` | In đậm (Bold) |
| `Ctrl + I` | In nghiêng (Italic) |
| `Ctrl + U` | Gạch chân (Underline) |
| `Ctrl + Z` / `Cmd + Z` | Hoàn tác (Undo) |
| `Ctrl + Y` / `Cmd + Shift + Z` | Làm lại (Redo) |
| `Enter` | Thêm mục mới trong Checklist |
| `Esc` | Xóa ô tìm kiếm trong nội dung ghi chú |

---

## Kiểm thử (Testing)

Chạy toàn bộ unit test bằng `unittest`:

```bash
python -m unittest discover -s tests
```

---

## Đóng gói ứng dụng

Dự án đã khai báo `pyinstaller>=6.0.0` trong `requirements.txt` để hỗ trợ đóng gói thành tệp thực thi độc lập khi cần:

```bash
pyinstaller --noconfirm --windowed --name EngraverNoteApp main.py
```

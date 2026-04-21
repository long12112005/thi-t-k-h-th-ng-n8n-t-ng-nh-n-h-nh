# n8n Telegram Image Edit Automation

Hệ thống tự động nhận ảnh và yêu cầu chỉnh sửa từ Telegram, xử lý bằng n8n, gọi API chỉnh sửa ảnh, sau đó lưu kết quả vào Google Drive với tên file lấy từ đoạn đề xuất/prompt đã rút gọn.

## 1. Mục tiêu
- Nhận ảnh từ Telegram bot.
- Nhận nội dung yêu cầu chỉnh sửa đi kèm ảnh.
- Tải ảnh gốc từ Telegram.
- Gọi API chỉnh sửa ảnh theo prompt.
- Đặt tên file kết quả theo prompt rút gọn.
- Upload ảnh kết quả lên Google Drive.
- Gửi thông báo hoàn tất về Telegram.

## 2. Kiến trúc
Telegram User -> Telegram Trigger -> n8n -> Telegram API Download -> Image Edit API -> Google Drive Upload -> Telegram Response

## 3. Chuẩn bị
- Tài khoản Telegram
- Bot Telegram tạo bằng BotFather
- n8n (cloud hoặc self-host)
- Google Drive credentials
- OpenAI API key
- Thư mục Google Drive đích

## 4. Biến môi trường gợi ý
Tạo file `.env` nếu bạn tự host:
```env
OPENAI_API_KEY=YOUR_OPENAI_API_KEY
TELEGRAM_BOT_TOKEN=YOUR_TELEGRAM_BOT_TOKEN
GOOGLE_DRIVE_FOLDER_ID=YOUR_GOOGLE_DRIVE_FOLDER_ID
```

## 5. Cấu trúc dự án
```text
.
├── README.md
├── docs/
│   └── report-outline.md
├── assets/
│   └── so_do_khoi.png
└── workflows/
    └── telegram-image-edit-drive.json
```

## 6. Cách chạy
1. Import file workflow `workflows/telegram-image-edit-drive.json` vào n8n.
2. Tạo các credentials:
   - Telegram Bot API
   - Google Drive OAuth2
   - HTTP Header Auth hoặc dùng biến môi trường cho OpenAI API
3. Sửa các giá trị:
   - `YOUR_GOOGLE_DRIVE_FOLDER_ID`
   - `YOUR_BOT_TOKEN`
4. Bật workflow.
5. Gửi cho bot:
   - 1 ảnh
   - caption ví dụ: `xóa nền, làm sáng mặt, đổi áo thành màu xanh`

## 7. Quy ước đặt tên file
Tên file được sinh từ prompt rút gọn:
- bỏ ký tự đặc biệt
- thay khoảng trắng bằng dấu `_`
- cắt tối đa 60 ký tự
- thêm timestamp

Ví dụ:
`xoa_nen_lam_sang_mat_doi_ao_mau_xanh_2026-04-21_101500.png`

## 8. Kết quả đầu ra
- Ảnh kết quả nằm trong thư mục Google Drive đã chọn
- Telegram nhận thông báo thành công
- Có thể gửi kèm link Google Drive hoặc ID file

## 9. Link GitHub nộp bài
Thay link dưới đây bằng repo thật của bạn:
`https://github.com/<your-username>/n8n-telegram-image-edit`

## 10. Lưu ý
- Nếu dùng n8n self-host với Telegram Trigger, webhook phải truy cập được từ Internet.
- Nếu ảnh lớn, cần kiểm tra giới hạn kích thước và timeout.
- Có thể thêm node IF để bắt lỗi khi người dùng gửi thiếu ảnh hoặc thiếu prompt.

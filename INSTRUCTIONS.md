# Hướng dẫn triển khai NocoBase bằng Docker trên VPS

Tài liệu này hướng dẫn bạn cách cài đặt và chạy NocoBase trên server VPS sử dụng Docker và Docker Compose.

## 1. Chuẩn bị

Trước khi bắt đầu, hãy đảm bảo VPS của bạn đã cài đặt:
- **Docker**
- **Docker Compose**

## 2. Các bước cài đặt

### Bước 1: Sao chép các tệp cấu hình vào VPS
Tạo một thư mục mới cho NocoBase và sao chép các tệp `docker-compose.yml` và `.env.example` vào đó.

### Bước 2: Cấu hình biến môi trường
Tạo tệp `.env` từ tệp mẫu:
```bash
cp .env.example .env
```

Mở tệp `.env` và thay đổi các thông tin quan trọng:
- `DB_PASSWORD` & `POSTGRES_PASSWORD`: Mật khẩu cơ sở dữ liệu (nên đặt phức tạp).
- `APP_KEY`: Một chuỗi ký tự ngẫu nhiên (ít nhất 32 ký tự) để bảo mật ứng dụng.
- `APP_PORT`: Cổng truy cập NocoBase (mặc định là 13000).

### Bước 3: Chạy Docker Compose
Khởi động các dịch vụ:
```bash
docker-compose up -d
```

### Bước 4: Khởi tạo ứng dụng (Chỉ thực hiện lần đầu)
Sau khi các container đã chạy, bạn cần chạy lệnh cài đặt NocoBase để khởi tạo database và tài khoản admin:
```bash
docker-compose exec nocobase yarn nocobase install --lang=vi-VN
```
*Lưu ý: Lệnh này sẽ yêu cầu bạn thiết lập email và mật khẩu cho tài khoản admin.*

### Bước 5: Kiểm tra trạng thái
Kiểm tra xem các container đã chạy ổn định chưa:
```bash
docker-compose ps
```

## 3. Truy cập NocoBase

- **NocoBase**: `http://<IP-SERVER>:<APP_PORT>` (Mặc định: `http://localhost:13000`)
- **Adminer (Quản lý DB)**: `http://<IP-SERVER>:<ADMINER_PORT>` (Mặc định: `http://localhost:8080`)

## 4. Các lệnh quản lý cơ bản

- **Dừng dịch vụ**: `docker-compose down`
- **Xem log**: `docker-compose logs -f nocobase`
- **Cập nhật NocoBase**:
  ```bash
  docker-compose pull
  docker-compose up -d
  ```

## 5. Lưu ý quan trọng
- Thư mục `./storage` sẽ được tạo tự động để lưu trữ dữ liệu. Hãy đảm bảo backup thư mục này thường xuyên.
- Nếu bạn sử dụng Firewall (như `ufw` trên Ubuntu), hãy mở các cổng tương ứng (`13000`, `8080`).

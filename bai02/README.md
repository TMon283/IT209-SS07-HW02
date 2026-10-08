# Bài 2: Quản trị Tường lửa UFW cho Cụm Dịch vụ Multi-port

## 1. Mục tiêu

Cấu hình UFW để bảo vệ máy chủ chạy nhiều dịch vụ:

- SSH: port 22
- Nginx: port 80
- Spring Boot: port 8082
- MySQL: port 3306

Chính sách mặc định là chặn toàn bộ kết nối đi vào và cho phép kết nối đi ra.

## 2. Cấu hình UFW

### Thiết lập chính sách mặc định

```bash
ufw default deny incoming
ufw default allow outgoing
```

### Cho phép SSH

```bash
ufw allow 22/tcp
```

### Cho phép HTTP

```bash
ufw allow 80/tcp
```

### Cho phép Spring Boot

```bash
ufw allow 8082/tcp
```

### Không mở MySQL

Không tạo rule `allow` cho port `3306/tcp`, do đó port này bị chặn bởi chính sách `deny incoming`.

## 3. Kiểm tra

### Command

```bash
ufw status verbose
```

### Terminal Output

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
8082/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp                     ALLOW IN    Anywhere (v6)
443/tcp                    ALLOW IN    Anywhere (v6)
8082/tcp                   ALLOW IN    Anywhere (v6)
```

## 4. Kết quả

- UFW đang ở trạng thái `active`.
- Incoming mặc định bị từ chối.
- Port `22/tcp` được phép cho SSH.
- Port `80/tcp` được phép cho HTTP.
- Port `8082/tcp` được phép cho ứng dụng Spring Boot.
- Port `3306/tcp` không có rule `ALLOW`, nên bị chặn từ Internet.
- Port `443/tcp` được giữ lại từ cấu hình trước đó.

## 5. Kết luận

Tường lửa UFW đã được cấu hình thành công để bảo vệ máy chủ trước các kết nối không được phép từ Internet, đồng thời chỉ cho phép các cổng dịch vụ cần thiết.
# Session 03 - Bài 1: Thay đổi cổng kết nối SSH (SSH Port Hardening)

## 1. Mục tiêu
- Thay đổi cổng dịch vụ SSH mặc định từ `22` sang `2222` nhằm hạn chế tối đa các cuộc quét cổng tự động (botnet scan).
- Cấu hình tường lửa UFW mở cổng `2222/tcp` trước khi restart SSH service để tránh bị khóa quyền truy cập (Lockout).

## 2. Các bước thực hiện (bằng tài khoản `devops`)

### Bước 1: Mở cổng 2222 trên tường lửa UFW trước
```bash
sudo ufw allow 2222/tcp
sudo ufw status
```

### Bước 2: Chỉnh sửa tệp cấu hình SSH Daemon `/etc/ssh/sshd_config`
```bash
sudo nano /etc/ssh/sshd_config
```
Tìm chỉ thị `Port 22` hoặc `#Port 22`, chỉnh sửa lại thành:
```config
Port 2222
```

### Bước 3: Restart dịch vụ SSH
```bash
sudo systemctl restart ssh
```

### Bước 4: Xóa luật cổng 22 cũ trên UFW
```bash
sudo ufw delete allow 22/tcp
```

## 3. Kết quả kiểm tra (Terminal Log)

### Kiểm tra 1: Kết nối qua cổng mới 2222
```bash
ssh -p 2222 devops@128.199.200.100
```
**Kết quả:** Đăng nhập thành công qua SSH Key.

### Kiểm tra 2: Kết nối qua cổng cũ 22
```bash
ssh -p 22 devops@128.199.200.100
```
**Kết quả:**
```text
ssh: connect to host 128.199.200.100 port 22: Connection refused
```
Cổng 22 đã bị ngắt kết nối hoàn toàn.

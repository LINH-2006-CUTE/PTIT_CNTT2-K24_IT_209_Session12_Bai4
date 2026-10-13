# IT209 Session12 - Bài 4: Tự động triển khai Spring Boot lên VPS

## 1. Thông tin bài tập

- **Môn học:** IT209
- **Bài:** Session12 - Bài 4
- **Nội dung:** Tự động build và triển khai ứng dụng Spring Boot lên VPS bằng Docker và GitHub Actions.
- **Repository:** https://github.com/LINH-2006-CUTE/PTIT_CNTT2-K24_IT_209_Session12_Bai4

## 2. Công nghệ sử dụng

- Java 17
- Spring Boot 3.5.6
- Gradle
- Docker
- GitHub Actions
- GitHub Container Registry (GHCR)
- SSH
- Ubuntu 24.04 LTS

## 3. Quy trình CI/CD

1. Đẩy mã nguồn lên nhánh `main` của GitHub.
2. GitHub Actions checkout mã nguồn.
3. Thiết lập môi trường Java và Gradle.
4. Build ứng dụng bằng Gradle.
5. Build Docker image và đẩy image lên GHCR.
6. SSH đến VPS.
7. Đăng nhập GHCR và tải Docker image mới.
8. Khởi chạy container `deploy-demo`.
9. Kiểm tra ứng dụng qua endpoint `/actuator/health`.

## 4. Các file quan trọng

- `.github/workflows/deploy-vps.yml`: workflow tự động build và triển khai.
- `Dockerfile`: cấu hình đóng gói ứng dụng thành Docker image.
- `build.gradle`: cấu hình build Gradle.
- `settings.gradle`: cấu hình dự án Gradle.
- `src/`: mã nguồn ứng dụng Spring Boot.

## 5. Cấu hình GitHub Secrets

Workflow sử dụng các secrets:

- `SSH_HOST`: địa chỉ VPS.
- `SSH_USER`: tài khoản SSH.
- `SSH_KEY`: khóa riêng dùng để SSH.
- `GHCR_USERNAME`: tài khoản GitHub dùng để truy cập GHCR.
- `GHCR_TOKEN`: token truy cập GHCR.

Không đưa khóa SSH hoặc token thật vào repository.

## 6. Kết quả kiểm thử thực tế

### 6.1. GitHub Actions

Workflow đã chạy thành công với hai job Build và Deploy.

### 6.2. Container trên VPS

Container `deploy-demo` đang chạy trên VPS, ánh xạ cổng 8080 của container ra cổng 8080 của máy chủ.

### 6.3. Health Check

Đã kiểm tra ứng dụng qua SSH tunnel bằng lệnh:

```powershell
curl.exe -i http://127.0.0.1:8081/actuator/health
```

Kết quả thực tế:

```text
HTTP/1.1 200
{"status":"UP"}
```

Ứng dụng phản hồi thành công qua đường hầm SSH.

**Lưu ý:** Truy cập trực tiếp từ Windows đến `http://221.121.0.64:8080/actuator/health` chưa thành công tại thời điểm kiểm thử. Vì vậy, chưa xác nhận được khả năng truy cập công khai vào cổng 8080; cần kiểm tra thêm mạng hoặc firewall của nhà cung cấp VPS.

## 7. Ảnh minh chứng

Lưu ảnh chụp kết quả thực tế trong thư mục `evidence/`.

| Tên file | Nội dung |
|---|---|
| `01-github-actions-success.png` | GitHub Actions với Build và Deploy thành công |
| `02-deploy-log.png` | Log triển khai trên VPS |
| `03-vps-container.png` | Kết quả kiểm tra container trên VPS |
| `04-health-check.png` | Kết quả HTTP 200 và trạng thái UP |

## 8. Kết luận

Bài tập đã triển khai được quy trình CI/CD sử dụng GitHub Actions, Docker, GHCR và SSH để build và triển khai ứng dụng Spring Boot lên VPS. Health Check trả về HTTP 200 với trạng thái `UP` khi kiểm tra qua SSH tunnel.

Khả năng truy cập trực tiếp từ Internet vào cổng 8080 vẫn cần được kiểm tra và hoàn thiện riêng.

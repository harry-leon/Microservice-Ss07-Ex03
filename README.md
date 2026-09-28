# Bài Tập 3 - Quản lý cấu hình theo môi trường (Profiles)

## 1. Mô tả dự án
Dự án minh họa việc sử dụng **Spring Cloud Config Server** để quản lý cấu hình đa môi trường (Profiles) cho dịch vụ `patient-service`:
- **Profile Dev**: Chạy ở cổng `8081`, kết nối cơ sở dữ liệu **MySQL**.
- **Profile Prod**: Chạy ở cổng `8082`, kết nối cơ sở dữ liệu **PostgreSQL**.

---

## 2. Cấu trúc thư mục
- `medical-config-repo/`: Chứa các file cấu hình Git cho Config Server:
  - `patient-service-dev.properties` (Cấu hình port 8081, MySQL)
  - `patient-service-prod.properties` (Cấu hình port 8082, PostgreSQL)
- `config-server/`: Service Spring Cloud Config Server (Chạy ở cổng `8888`).
- `patient-service/`: Client Microservice nhận cấu hình động từ Config Server dựa trên profile active (`dev` hoặc `prod`).

---

## 3. Các bước thực hiện & Kiểm tra

### Bước 1: Khởi tạo Config Repo (Git Repository)
1. Tạo thư mục `C:/medical-config-repo` (hoặc sử dụng folder `medical-config-repo` trong project).
2. Tạo 2 file cấu hình:
   - `patient-service-dev.properties`
   - `patient-service-prod.properties`
3. Thực hiện `git init`, `git add .`, và `git commit` trong repository cấu hình.

### Bước 2: Chạy Config Server (Port 8888)
Chạy ứng dụng `config-server`. Kiểm tra các URL sau trên trình duyệt hoặc Postman:
- Dev Profile: `http://localhost:8888/patient-service/dev`
- Prod Profile: `http://localhost:8888/patient-service/prod`

### Bước 3: Cấu hình Client (`patient-service`)
File `application.properties` của `patient-service`:
```properties
spring.application.name=patient-service
spring.config.import=optional:configserver:http://localhost:8888
```

---

## 4. Chạy ứng dụng Patient Service theo Profile

### Cách 1: Chạy Profile Dev
Thêm tham số: `--spring.profiles.active=dev`
- **Kết quả**: Ứng dụng khởi chạy trên cổng **8081** và kết nối cơ sở dữ liệu **MySQL**.

### Cách 2: Chạy Profile Prod
Thêm tham số: `--spring.profiles.active=prod`
- **Kết quả**: Ứng dụng khởi chạy trên cổng **8082** và kết nối cơ sở dữ liệu **PostgreSQL**.

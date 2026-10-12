# Bài 2 - Đóng gói Web Backend bằng Single-stage Dockerfile

Ứng dụng REST API Node.js/Express được đóng gói bằng Dockerfile single-stage (base image `node:18-alpine`).

## Cấu trúc

```
.
├── Dockerfile
├── .dockerignore
├── package.json
├── server.js
└── README.md
```

## Các chỉ thị trong Dockerfile

| Chỉ thị | Giá trị | Ý nghĩa |
|---------|---------|---------|
| `FROM` | `node:18-alpine` | Base image Node.js 18 gọn nhẹ |
| `WORKDIR` | `/app` | Thư mục làm việc trong container |
| `COPY` | `package*.json ./` | Copy manifest trước để tận dụng cache layer |
| `RUN` | `npm install --omit=dev` | Cài dependencies production |
| `COPY` | `. .` | Copy toàn bộ mã nguồn |
| `ENV` | `PORT=3000` | Biến môi trường cổng |
| `EXPOSE` | `3000` | Khai báo cổng lắng nghe |
| `CMD` | `["node", "server.js"]` | Exec form chạy ứng dụng |

## Build

```bash
docker build -t hw-backend:v1 .
```

## Chạy container

```bash
docker run -d --name app-single-stage -p 8080:3000 hw-backend:v1
```

## Kiểm tra

```bash
docker ps
docker logs app-single-stage
curl -i http://localhost:8080/
```

Kết quả mong đợi: HTTP `200 OK` với response JSON:

```json
{ "status": "success", "message": "Hello from Docker Single-stage!" }
```

## Dọn dẹp

```bash
docker rm -f app-single-stage
docker rmi hw-backend:v1
```

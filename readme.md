# Hệ Thống Quản Lý chấm công (Attendance System Management)

## Tổng Quan
Dự án là một hệ thống quản lý chấm công toàn diện, được xây dựng với mục tiêu số hóa quy trình quản lý nhân sự, theo dõi thời gian làm việc và quản lý các yêu cầu nghỉ phép. Hệ thống bao gồm đầy đủ các tính năng cho Admin, Manager và Nhân viên.

### Công Nghệ Sử Dụng (Tech Stack)
-   **Frontend**: ReactJS, TailwindCSS.
-   **Backend**: Express.js.
-   **Database**: MongoDB.
-   **Containerization**: Docker.

---

## Các Công Cụ Cần Thiết
1.  **Docker Desktop**: Dùng để chạy môi trường server và database.
    -   [Tải Docker Desktop](https://www.docker.com/products/docker-desktop/)
2.  **MongoDB Compass**: Giao diện trực quan để quản lý và xem dữ liệu MongoDB.
    -   [Tải MongoDB Compass](https://www.mongodb.com/products/tools/compass)

---

## Hướng Dẫn Cài Đặt & Chạy Dự Án

### 1. Khởi chạy hệ thống với Docker

```bash
docker compose up --build
```

### 2. Khởi tạo tài khoản Admin
*Tài khoản mặc định: `admin` / `123123`*

```bash
docker compose exec server node src/utils/setupAdmin.js
```

### 3. Cộng thêm ngày nghỉ phép (Thủ công)
Hệ thống có cronjob chạy tự động, nhưng bạn có thể chạy thủ công lệnh cộng thêm ngày phép cho tất cả nhân viên trong hệ thống:

```bash
docker compose exec server node src/utils/calculateLeaveDays.js
```

### 4. Tạo dữ liệu check-in mẫu (Seeding Data)
*Lưu ý: Lệnh này sẽ tạo dữ liệu chấm công cho các nhân viên (không phải admin) từ tháng 6/2025 đến hiện tại.*

```bash
docker compose exec server node src/utils/seedAttendance.js
```

---

## Quy Trình Test Chức Năng (User Flows)

### 1. Khởi tạo dữ liệu cơ bản
1.  Đăng nhập bằng tài khoản Admin (`admin`/`123123`).
2.  **Tạo Phòng Ban (Groups)**: Vào quản lý phòng ban -> Thêm mới 
*Ví dụ: Ban Giám đốc (quyền phê duyệt request), Division 1, Division 2 (quyền xác nhận request, group cha là Ban Giám Đốc), *.
3.  **Tạo Nhân Viên**: Vào quản lý nhân viên -> Thêm mới user -> Gán vào phòng ban tương ứng.
4.  **Thêm người quản lý phòng ban**: Vào quản lý phòng ban -> Thêm người quản lý cho các phòng ban.

### 2. Kiểm tra quy trình Nghỉ Phép (Leave Request)
1.  **Tạo request**: Đăng nhập tài khoản Nhân viên -> Tạo đơn xin nghỉ phép.
2.  **Duyệt request (Manager)**: Đăng nhập tài khoản trưởng phòng đang quản lý nhân viên -> Duyệt request của nhân viên thuộc phòng ban mình -> Đăng nhập tài khoản CEO cấp cao hơn -> Duyệt request của nhân viên mà trưởng phòng đã xác nhận.
3.  **Duyệt request (Admin)**: Admin có quyền duyệt tất cả đơn.

### 3. Kiểm tra Chấm Công (Attendance)
1.  Sử dụng chức năng "Điểm danh" (Check-in) và "Kết thúc" (Check-out) trên giao diện Dashboard.
2.  Kiểm tra lịch sử chấm công trong mục "Lịch làm việc".
3.  Admin có thể xem chấm công của toàn bộ nhân viên.

### 4. Kiểm tra Tin Tức (News)
1.  Admin đăng bài viết thông báo mới.
2.  Nhân viên nhận được thông báo và xem bài viết trên trang chủ.

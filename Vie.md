# Book4u — Hệ thống Website Bán Sách Trực Tuyến

[Tiếng Việt](Vie.md) | [English](english.md)

Book4u là hệ thống website thương mại điện tử chuyên biệt về sách được xây dựng theo mô hình kiến trúc **Custom MVC (PHP thuần)**, phục vụ khách hàng tìm kiếm, đặt mua sách trực tuyến và cung cấp bảng điều khiển quản trị toàn diện cho việc quản lý kho sách, đơn hàng, khách hàng và thống kê doanh thu.

---

## Tính năng

### 1. Quản lý tài khoản & Xác thực

- Đăng ký tài khoản khách hàng mới với email, họ tên, số điện thoại; mã hóa mật khẩu một chiều bằng bcrypt (`password_hash`).
- Bắt buộc xác nhận đồng thuận **Điều khoản sử dụng** trước khi tạo tài khoản hoặc đăng nhập.
- Đăng nhập bảo mật, kiểm tra trạng thái kích hoạt tài khoản (`is_active`); hỗ trợ phản hồi JSON đồng bộ với AJAX frontend.
- Khôi phục mật khẩu hai bước: Tạo mã OTP ngẫu nhiên 6 chữ số có hiệu lực 15 phút, gửi email tự động qua **PHPMailer**.
- Trang cá nhân khách hàng (Dashboard): Cập nhật thông tin liên hệ, tải lên ảnh đại diện (avatar), quản lý sổ địa chỉ giao hàng và đổi mật khẩu.

### 2. Duyệt sách & Tìm kiếm thời gian thực

- Trang chủ phân khu trực quan: Top sách bán chạy (`total_sold DESC`), sách mới phát hành (`created_at DESC`), danh mục nổi bật và khối dịch vụ cam kết.
- Tìm kiếm thời gian thực (**Live Search AJAX**) ngay trên thanh điều hướng, tự động gợi ý kết quả và lưu lịch sử tìm kiếm gần nhất.
- Bộ lọc đa tiêu chí: Lọc theo danh mục, khoảng giá; sắp xếp theo độ bán chạy, giá tăng/giảm, điểm đánh giá; hỗ trợ phân trang chuẩn 12 sách/trang.
- Chi tiết sản phẩm: Gallery ảnh bìa và nhiều ảnh xem trước (thumbnail slider), thông số xuất bản chi tiết (ISBN, NXB, năm XB, số trang, kích thước).

### 3. Giỏ hàng, Đặt hàng & Đánh giá

- Quản lý giỏ hàng: Thêm sách, cập nhật số lượng linh hoạt, tự động cộng dồn số lượng sách đã có.
- Kiểm tra tồn kho trước khi thanh toán: Chặn đặt hàng vượt quá số lượng khả dụng thực tế trong kho (`stock_quantity`).
- Tự động áp dụng chính sách vận chuyển: Miễn phí vận chuyển cho đơn hàng từ `300.000₫` trở lên; áp dụng phí mặc định `30.000₫` cho các đơn còn lại.
- Hỗ trợ đa dạng phương thức thanh toán: COD (thanh toán khi nhận hàng), Chuyển khoản ngân hàng, Ví điện tử và Thẻ tín dụng.
- Quản lý đơn hàng người mua: Xem lịch sử chi tiết; cho phép **Hủy đơn hàng** hoặc **Đổi địa chỉ giao hàng** khi đơn còn ở trạng thái *Chờ xác nhận* (`pending`).
- Đánh giá sản phẩm: Viết nhận xét, chấm điểm sao, hỗ trợ chỉnh sửa và xóa đánh giá (chỉ áp dụng đối với khách hàng đã mua và nhận hàng thành công).

### 4. Quản trị hệ thống & Báo cáo doanh thu

- Dashboard quản trị tổng quan: Thống kê tổng số sách, khách hàng, đơn hàng và tổng doanh thu thực nhận từ các đơn đã giao (`delivered`).
- Biểu đồ doanh thu 12 tháng gần nhất tích hợp **Chart.js** thông qua REST API nội bộ (`/admin/revenue-data`).
- Quản lý kho sách (CRUD): Thêm/sửa/xóa sách, tải lên ảnh bìa đại diện và gallery ảnh xem trước, cập nhật giá bìa, giá bán và tồn kho.
- Quản lý danh mục: Phân cấp danh mục đa tầng (danh mục cha - con), tự động tạo URL slug thân thiện SEO.
- Quản lý đơn hàng: Cập nhật quy trình giao vận (*Chờ xác nhận* $\rightarrow$ *Đang xử lý* $\rightarrow$ *Đang giao* $\rightarrow$ *Đã giao* / *Đã hủy*); tự động đồng bộ trạng thái thanh toán.
- Quản lý người dùng: Xem danh sách tài khoản, kích hoạt hoặc tạm khóa tài khoản vi phạm.

### 5. An toàn dữ liệu & Kiến trúc hệ thống

- Xây dựng theo mô hình **Custom MVC** gọn nhẹ, phân tách độc lập giữa Controller, Model và View, không phụ thuộc framework bên thứ ba.
- Toàn bộ truy vấn CSDL sử dụng **PDO Prepared Statements** ngăn chặn triệt để lỗ hổng SQL Injection.
- Thiết kế CSDL quan hệ chặt chẽ trên MySQL InnoDB: Ràng buộc khóa ngoại (`FOREIGN KEY`), khóa duy nhất (`UNIQUE`), chỉ mục (`INDEX`) và hành vi cascade an toàn (`ON DELETE CASCADE` / `SET NULL`).
- Kiến trúc **Front Controller** duy nhất qua `public/index.php` kết hợp Apache URL Rewrite (`.htaccess`).

---

## Công nghệ

- **Ngôn ngữ / Nền tảng:** PHP 8.3 (Custom MVC Framework)
- **Cơ sở dữ liệu:** MySQL 8.4 (PDO, Engine InnoDB, UTF-8 MB4)
- **Giao diện / Frontend:** HTML5 Semantic, CSS3 Modular, Vanilla JavaScript (ES6 Modules)
- **Thư viện UI / Đồ họa:** Chart.js, Font Awesome 6, Google Fonts (Inter, Outfit)
- **Dịch vụ Email:** PHPMailer (SMTP OTP Authentication)
- **Môi trường máy chủ:** Apache (Laragon / XAMPP)

---

## Yêu cầu môi trường

- **PHP:** Phiên bản 8.2 trở lên (khuyến nghị PHP 8.3) với các extension: `pdo_mysql`, `mbstring`, `openssl`, `curl`
- **MySQL:** Phiên bản 8.0 trở lên (hoặc MariaDB 10.5+)
- **Web Server:** Apache bật module `mod_rewrite`

---

## Cài đặt và Chạy thử

### 1. Tải mã nguồn

```bash
git clone https://github.com/M4NH693/Bookstore-Website.git
cd Bookstore-Website
```

### 2. Khởi tạo Cơ sở dữ liệu

Tạo cơ sở dữ liệu `bookstore` và nhập dữ liệu theo thứ tự:

```bash
mysql -u root -p -e "CREATE DATABASE bookstore CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
mysql -u root -p bookstore < database/bookstore.sql
mysql -u root -p bookstore < database/seed_data.sql
```

### 3. Cấu hình kết nối

Mở file `app/config/database.php` và điều chỉnh thông số cho phù hợp với môi trường của bạn:

```php
return [
    'host'     => 'localhost',
    'dbname'   => 'bookstore',
    'username' => 'root',
    'password' => '',
    'charset'  => 'utf8mb4'
];
```

### 4. Khởi chạy ứng dụng

- **Dùng Laragon / XAMPP (Khuyến nghị):**
  - Đặt thư mục dự án vào `C:/laragon/www/Bookstore-Website` (hoặc `C:/xampp/htdocs/Bookstore-Website`).
  - Khởi động Apache & MySQL trên bảng điều khiển.
  - Truy cập trình duyệt theo đường dẫn: `http://localhost/Bookstore-Website/public` hoặc Virtual Host cấu hình riêng.

- **Dùng PHP Built-in Server (Thử nghiệm nhanh):**
  ```bash
  php -S localhost:8000 -t public
  ```
  Truy cập: `http://localhost:8000`

---

## Tài khoản thử nghiệm

Dữ liệu mẫu (`database/seed_data.sql`) cung cấp sẵn các tài khoản sau:

| Quyền hạn | Email | Mật khẩu mặc định | Ghi chú |
| :--- | :--- | :--- | :--- |
| **Quản trị viên (Admin)** | `admin@bookstore.vn` | `admin123` | Toàn quyền truy cập Dashboard `/admin` |
| **Khách hàng (Customer)** | `nguyenvana@gmail.com` | `password` | Tài khoản khách hàng mẫu |
| **Khách hàng (Customer)** | `tranthib@gmail.com` | `password` | Tài khoản khách hàng mẫu |

---

## Cấu trúc thư mục

```text
Bookstore-Website/
├── app/
│   ├── config/          # Cấu hình CSDL và môi trường
│   ├── controllers/     # Bộ điều khiển xử lý nghiệp vụ (Admin, Auth, Book, Cart, Order...)
│   ├── core/            # Hạt nhân MVC (Router, Controller, Model, Database, Mailer)
│   ├── models/          # Lớp tương tác CSDL (User, Book, Category, Cart, Order)
│   └── views/           # Giao diện PHP theo từng chức năng và layout
├── database/            # Script tạo bảng CSDL và seed dữ liệu mẫu
├── public/              # Thư mục web gốc công khai (index.php, CSS, JS modules, ảnh)
│   ├── css/             # CSS phân theo components, pages, admin
│   ├── js/modules/      # JavaScript chia theo ES6 modules (cart, search, auth...)
│   └── images/          # Thư mục lưu trữ hình ảnh sách, avatar
├── SRS.md               # Tài liệu đặc tả yêu cầu phần mềm chi tiết (SRS v2.2)
└── .htaccess            # Cấu hình URL Rewrite vào thư mục public
```

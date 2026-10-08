
---

### 📄 Nội dung `README.md` (English)

```markdown
# Book4u — Online Bookstore Management System

[English](README.md) | [Tiếng Việt](README_VI.md)

Book4u is a specialized e-commerce web application for books built with a **Custom MVC architecture in vanilla PHP**. It enables customers to discover, search, and purchase books online, while providing administrators with a full-featured management panel for catalog maintenance, inventory tracking, order processing, and revenue reporting.

---

## Features

### 1. Authentication & Account Management

- Customer registration with email uniqueness validation and bcrypt password hashing (`password_hash`).
- Mandatory **Terms of Service** agreement prior to registration and login.
- Secure session-based authentication verifying account status (`is_active`); full AJAX/JSON support for smooth UI feedback.
- Two-step password recovery: Generates a 6-digit random OTP valid for 15 minutes, dispatched automatically via **PHPMailer**.
- Customer Profile Dashboard: Manage personal details, upload avatars, configure shipping addresses, and change passwords.

### 2. Catalog & Real-Time Search

- Rich homepage sections: Best Sellers (`total_sold DESC`), New Arrivals (`created_at DESC`), featured categories, and service commitments.
- Real-time search (**Live Search AJAX**) in the top navigation bar with auto-suggestions and recent search history tracking.
- Multi-criteria filtering: Filter by categories and price range; sort by popularity, price ascending/descending, and average rating; 12 items/page pagination.
- Detailed book view: High-resolution cover gallery with thumbnail slider, comprehensive publishing specifications (ISBN, publisher, year, pages, dimensions).

### 3. Cart, Checkout & Reviews

- Shopping cart: Add items, update quantities dynamically, and prevent duplicate cart lines.
- Pre-order inventory check: Blocks order creation when requested items exceed warehouse stock (`stock_quantity`).
- Automated shipping policy: Free shipping for orders equal to or exceeding `300,000₫`; standard `30,000₫` fee applied otherwise.
- Flexible payment methods: Cash On Delivery (COD), Direct Bank Transfer, E-Wallets, and Credit Cards.
- Order tracking: View order history and line-item details; allow buyers to **Cancel Order** or **Update Shipping Address** while in *Pending* status.
- Verified customer reviews: Submit ratings and text feedback with edit/delete support (restricted to verified delivered orders).

### 4. Admin Panel & Analytics

- Administrative dashboard: At-a-glance metrics for total books, registered customers, orders, and realized revenue from delivered orders.
- 12-month revenue analytics powered by **Chart.js** via internal JSON endpoint (`/admin/revenue-data`).
- Catalog management (CRUD): Create, update, soft-delete books, upload cover pictures and preview gallery images, adjust pricing and inventory.
- Category management: Multi-level hierarchy (parent-child categories) with automatic SEO-friendly URL slug generation.
- Order fulfillment: Step-by-step lifecycle workflow (*Pending* $\rightarrow$ *Processing* $\rightarrow$ *Shipping* $\rightarrow$ *Delivered* / *Cancelled*) with automatic payment status synchronization.
- User management: Monitor user accounts, toggle access states (`is_active`).

### 5. Data Safety & Architecture

- Clean, lightweight **Custom MVC** engine without heavy third-party framework dependencies.
- 100% parameter-bound queries using **PDO Prepared Statements** to eliminate SQL Injection risks.
- Robust relational schema on MySQL InnoDB: Enforced foreign key constraints (`FOREIGN KEY`), unique indices, and safe cascade rules (`ON DELETE CASCADE` / `SET NULL`).
- Single **Front Controller** pattern via `public/index.php` coordinated with Apache `.htaccess` rewrite rules.

---

## Technology Stack

- **Language / Platform:** PHP 8.3 (Custom MVC Framework)
- **Database:** MySQL 8.4 (PDO, InnoDB Engine, UTF-8 MB4)
- **Frontend:** Semantic HTML5, Modular CSS3, Vanilla JavaScript (ES6 Modules)
- **UI / Chart Libraries:** Chart.js, Font Awesome 6, Google Fonts (Inter, Outfit)
- **Mailing Service:** PHPMailer (SMTP OTP Authentication)
- **Server Environment:** Apache (Laragon / XAMPP)

---

## Requirements

- **PHP:** 8.2 or higher (PHP 8.3 recommended) with extensions: `pdo_mysql`, `mbstring`, `openssl`, `curl`
- **MySQL:** 8.0 or higher (or MariaDB 10.5+)
- **Web Server:** Apache with `mod_rewrite` enabled

---

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/M4NH693/Bookstore-Website.git
cd Bookstore-Website

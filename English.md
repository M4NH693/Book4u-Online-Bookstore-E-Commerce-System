
# Book4u — Online Bookstore E-Commerce System

[English](english.md) | [Tiếng Việt](README.md)

Book4u is a specialized e-commerce web platform for books built with a **Custom MVC architecture in Vanilla PHP**. It enables customers to discover, search, and purchase books online, while providing administrators with a comprehensive control panel for catalog maintenance, warehouse inventory, order fulfillment, and revenue analytics.

---

## Features

### 1. Account Management & Authentication

- Customer registration with email uniqueness validation, contact information, and one-way password hashing via bcrypt (`password_hash`).
- Mandatory **Terms of Service** consent agreement prior to account creation and login.
- Secure session-based authentication verifying account active state (`is_active`); full AJAX/JSON support for smooth frontend interactions.
- Two-step password recovery: Generates a cryptographically random 6-digit OTP valid for 15 minutes, dispatched automatically via **PHPMailer**.
- Customer Profile Dashboard: Update personal details, upload custom avatars, manage address book, and change passwords.

### 2. Catalog & Real-Time Search

- Intuitive homepage layout: Best Sellers (`total_sold DESC`), New Arrivals (`created_at DESC`), featured categories, and trusted service commitments.
- Real-time search (**Live Search AJAX**) integrated into the main navigation bar with instant suggestions and recent search query history.
- Multi-criteria filtering: Filter by categories and price range; sort by popularity, price ascending/descending, and average customer rating; standardized pagination with 12 books per page.
- Comprehensive book detail view: Cover gallery with thumbnail slider, publishing metadata (ISBN, publisher, publication year, page count, dimensions).

### 3. Shopping Cart, Orders & Reviews

- Cart management: Add books, update item quantities dynamically, and automatically merge duplicate items.
- Pre-checkout stock verification: Rejects order placement when requested quantities exceed available warehouse stock (`stock_quantity`).
- Automated shipping policy calculation: Free shipping for orders equal to or exceeding `300,000₫`; standard `30,000₫` fee applied for remaining orders.
- Diverse payment options: Cash On Delivery (COD), Direct Bank Transfer, E-Wallets, and Credit Cards.
- Order lifecycle tracking: Inspect order history and line-item details; allow buyers to **Cancel Order** or **Change Shipping Address** while orders remain in *Pending* status.
- Verified customer reviews: Submit ratings and text feedback with edit and delete capabilities (exclusively restricted to verified delivered orders).

### 4. Admin Panel & Business Analytics

- Executive admin dashboard: High-level overview of total book catalog, registered customers, total orders, and realized revenue from delivered orders.
- 12-month historical revenue chart powered by **Chart.js** via an internal JSON REST endpoint (`/admin/revenue-data`).
- Book catalog management (CRUD): Add, edit, or toggle book visibility; upload book covers and multiple preview gallery images; adjust retail pricing and inventory.
- Category management: Multi-tier category hierarchy (parent-child categories) with automatic SEO-friendly URL slug generation.
- Order processing workflow: Step-by-step dispatch pipeline (*Pending* $\rightarrow$ *Processing* $\rightarrow$ *Shipping* $\rightarrow$ *Delivered* / *Cancelled*); automatic payment status synchronization.
- User management: Audit customer accounts, toggle account status (`is_active = 0` to ban).

### 5. Data Safety & System Architecture

- Lightweight **Custom MVC Framework** without reliance on bulky third-party dependencies, strictly isolating Controller, Model, and View layers.
- 100% parameter-bound queries using **PDO Prepared Statements** to eliminate SQL Injection vulnerabilities.
- Strongly-typed relational schema on MySQL InnoDB: Enforced foreign key constraints (`FOREIGN KEY`), unique indices, and safe cascade rules (`ON DELETE CASCADE` / `SET NULL`).
- Single **Front Controller** pattern via `public/index.php` paired with Apache URL Rewrite rules (`.htaccess`).

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

- **PHP:** Version 8.2 or higher (PHP 8.3 recommended) with extensions: `pdo_mysql`, `mbstring`, `openssl`, `curl`
- **MySQL:** Version 8.0 or higher (or MariaDB 10.5+)
- **Web Server:** Apache with `mod_rewrite` enabled

---

## Installation & Setup

### 1. Clone Repository

```bash
git clone https://github.com/M4NH693/Bookstore-Website.git
cd Bookstore-Website
```

### 2. Initialize Database

Create the `bookstore` database and import the SQL files in order:

```bash
mysql -u root -p -e "CREATE DATABASE bookstore CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
mysql -u root -p bookstore < database/bookstore.sql
mysql -u root -p bookstore < database/seed_data.sql
```

### 3. Configure Database Connection

Open `app/config/database.php` and configure your local credentials:

```php
return [
    'host'     => 'localhost',
    'dbname'   => 'bookstore',
    'username' => 'root',
    'password' => '',
    'charset'  => 'utf8mb4'
];
```

### 4. Run the Application

- **Using Laragon / XAMPP (Recommended):**
  - Move the project directory to `C:/laragon/www/Bookstore-Website` (or `C:/xampp/htdocs/Bookstore-Website`).
  - Start Apache & MySQL services.
  - Open your browser and navigate to: `http://localhost/Bookstore-Website/public` or your local Virtual Host.

- **Using PHP Built-in Server (Quick Preview):**
  ```bash
  php -S localhost:8000 -t public
  ```
  Visit: `http://localhost:8000`

---

## Test Accounts

The seed dataset (`database/seed_data.sql`) provides the following pre-configured credentials:

| Role | Email | Default Password | Notes |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@bookstore.vn` | `admin123` | Full administrative access to `/admin` |
| **Customer** | `nguyenvana@gmail.com` | `password` | Demo customer account |
| **Customer** | `tranthib@gmail.com` | `password` | Demo customer account |

---

## Directory Structure

```text
Bookstore-Website/
├── app/
│   ├── config/          # Database and application configuration
│   ├── controllers/     # Business logic controllers (Admin, Auth, Book, Cart, Order...)
│   ├── core/            # MVC framework core (Router, Controller, Model, Database, Mailer)
│   ├── models/          # Database model layer (User, Book, Category, Cart, Order)
│   └── views/           # PHP view templates, partials, and layouts
├── database/            # Database schema definitions and seed data
├── public/              # Public webroot (index.php, CSS stylesheets, JS modules, images)
│   ├── css/             # Component, page, and admin stylesheets
│   ├── js/modules/      # Modular ES6 JavaScript files (cart, search, auth...)
│   └── images/          # Uploaded media assets (book covers, gallery, user avatars)
├── SRS.md               # Software Requirements Specification (SRS v2.2)
└── .htaccess            # URL rewriting to public directory
```
````

# Car Rental Platform - Setup & Installation Guide

## 📋 Project Requirements

- **PHP:** 8.2.12 (or compatible version)
- **MySQL/MariaDB:** 5.6+ (for database)
- **Web Server:** PHP Built-in Server OR Apache/Nginx

---

## ✅ Prerequisites Checklist

- [ ] PHP 8.2+ installed and working
- [ ] MySQL/MariaDB server running
- [ ] Database access credentials available
- [ ] Git (for version control)

---

## 🚀 Quick Start (5 Steps)

### **Step 1: Setup Database**

#### Option A: Using phpMyAdmin (GUI)
1. Open phpMyAdmin in your browser: `http://localhost/phpmyadmin`
2. Click "New Database"
3. Enter database name: `carrental`
4. Click "Create"
5. Select the `carrental` database
6. Go to "Import" tab
7. Upload `carrental.sql` from the project root
8. Click "Import"

#### Option B: Using MySQL Command Line
```bash
mysql -u root -p < carrental.sql
```

Or if MySQL is in the PATH:
```bash
mysql -h localhost -u root -p carrental < carrental.sql
```

#### Option C: Import via PHP Script
```bash
cd C:\Users\HP\Desktop\new updation\CarRentalProject
php import_db.php
```

---

### **Step 2: Configure Database Connection**

Edit `config.php` in the project root:

```php
<?php 
define('DB_HOST','localhost');      // MySQL host
define('DB_USER','root');           // MySQL username
define('DB_PASS','');               // MySQL password
define('DB_NAME','carrental');      // Database name

try {
    $dbh = new PDO(
        "mysql:host=".DB_HOST.";dbname=".DB_NAME,
        DB_USER, 
        DB_PASS,
        array(PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'utf8'")
    );
} catch (PDOException $e) {
    exit("Error: " . $e->getMessage());
}
?>
```

**Update the following if needed:**
- `DB_HOST` - MySQL server address (default: `localhost`)
- `DB_USER` - MySQL username (default: `root`)
- `DB_PASS` - MySQL password (leave empty if no password)
- `DB_NAME` - Database name (must match imported database)

---

### **Step 3: Start PHP Server**

#### Option A: PHP Built-in Server (Recommended for Development)
```bash
cd C:\Users\HP\Desktop\new updation\CarRentalProject
php -S localhost:8000
```

Server will be available at: `http://localhost:8000`

#### Option B: Using Apache/Nginx
Configure virtual host to point to the project root directory.

**Apache Virtual Host Example:**
```apache
<VirtualHost *:80>
    ServerName carental.local
    DocumentRoot "C:\Users\HP\Desktop\new updation\CarRentalProject"
    
    <Directory "C:\Users\HP\Desktop\new updation\CarRentalProject">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

---

### **Step 4: Create Directories (If Missing)**

```bash
cd C:\Users\HP\Desktop\new updation\CarRentalProject
mkdir -p admin assets includes sqlfile
```

Ensure these directories have appropriate permissions:
```bash
# Windows: Already have default permissions
# Linux/Mac: chmod 755 admin assets includes sqlfile
```

---

### **Step 5: Access the Application**

#### User Portal
- **URL:** `http://localhost:8000`
- **Register:** Create new user account
- **Login:** Use registered email/password

#### Admin Panel
- **URL:** `http://localhost:8000/admin/`
- **Username:** `admin`
- **Password:** Hash: `5c428d8875d2948607f3e3fe134d71b4` (MD5 encrypted)
  - Default password must be reset (see below)

---

## 🔑 Admin Account Setup

### Option 1: Reset Admin Password via Database

Use phpMyAdmin or MySQL command:

```sql
-- Generate new password hash for "Admin@123"
UPDATE admin 
SET Password = MD5('Admin@123') 
WHERE UserName = 'admin';

-- Or use bcrypt (recommended for security):
-- Password: 'SecurePass123'
-- Hash: $2y$10$... (use online bcrypt generator or PHP script)
```

### Option 2: Direct SQL Update
```bash
MySQL Command:
mysql -u root -p carrental -e "UPDATE admin SET Password = MD5('NewPassword123') WHERE UserName = 'admin';"
```

---

## 📁 Project Structure

```
CarRentalProject/
├── admin/                    # Admin panel files (currently empty)
├── assets/                   # CSS, JS, fonts, images
├── includes/                 # Config, headers, footers, php files
├── sqlfile/                  # Database backup
├── index.php                 # Home page
├── car-listing.php          # Car list page
├── car-listing.php          # Search results
├── vehical-details.php      # Vehicle details page
├── my-booking.php           # User bookings
├── my-testimonials.php      # User testimonials
├── profile.php              # User profile
├── contact-us.php           # Contact form
├── config.php               # Database configuration
├── carrental.sql            # Database dump
└── README.md                # Project info
```

---

## 🔧 Common Issues & Troubleshooting

### Issue: "Error: SQLSTATE[HY000]: General error: 3 Error writing file"
**Solution:** Ensure MySQL temp directory has write permissions
```bash
MySQL: SET GLOBAL secure_file_priv = '';
```

### Issue: "Class 'PDO' not found"
**Solution:** Enable PHP PDO extension in `php.ini`
- Uncomment: `extension=pdo_mysql`
- Restart PHP server

### Issue: "Access denied for user 'root'@'localhost'"
**Solution:** Update config.php with correct MySQL credentials
```php
define('DB_USER','correct_username');
define('DB_PASS','your_password');
```

### Issue: "Cannot connect to MySQL server on 'localhost'"
**Solution:** 
1. Ensure MySQL service is running
2. Check MySQL port (default: 3306)
3. Update DB_HOST in config.php if needed

---

## 🎯 Quick Commands Reference

| Purpose | Command |
|---------|---------|
| **Start PHP Server** | `php -S localhost:8000` |
| **Import Database** | `mysql -u root -p < carrental.sql` |
| **Check PHP Version** | `php -v` |
| **Check PHP Extensions** | `php -m` |
| **Test DB Connection** | `php -r "new PDO('mysql:host=localhost;dbname=carrental','root','');"` |
| **Restart MySQL (Windows)** | `net stop MySQL80` then `net start MySQL80` |
| **Restart MySQL (Linux)** | `sudo systemctl restart mysql` |
| **View Error Logs** | Check `error_log` in project root |

---

## 🔐 Security Recommendations

1. **Change Admin Password:**
   ```sql
   UPDATE admin SET Password = MD5('StrongPassword123') WHERE UserName = 'admin';
   ```

2. **Update error_reporting in index.php:**
   - Remove `error_reporting(0)` in production
   - Change to: `error_reporting(E_ALL)`

3. **Upgrade Password Hashing:**
   - Replace MD5 with bcrypt (PHP 5.5+)
   - Use: `password_hash()` and `password_verify()`

4. **Enable HTTPS:**
   - Install SSL certificate
   - Update all URLs to use `https://`

---

## 📞 Database Credentials

**Current Configuration (config.php):**
```
Host:     localhost
User:     root
Password: (empty)
Database: carrental
Port:     3306 (default)
```

---

## ✨ After Setup

1. ✅ Verify homepage loads: `http://localhost:8000`
2. ✅ Test user registration
3. ✅ Test user login
4. ✅ Access admin panel: `http://localhost:8000/admin/`
5. ✅ Browse car listings
6. ✅ Test search functionality

---

## 📝 Additional Notes

- Project uses **PDO** for database connections (secure)
- **Session management** enabled for user authentication
- **HTML/Bootstrap** frontend responsive design
- Tables included: admin, tblusers, tblvehicles, tblbrands, tblbooking, tblreview, tblsubscribers, tblcontactusquery, etc.

---

**Last Updated:** March 19, 2026  
**Version:** 1.0  
**Repository:** https://github.com/Aniket033-gupta/Car-Rental-Platform-With-Real-Time-Availability

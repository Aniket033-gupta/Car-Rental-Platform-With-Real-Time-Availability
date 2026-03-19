# Car Rental Platform - Quick Commands

## 🚀 Start Application (Single Command)

```bash
cd C:\Users\HP\Desktop\new updation\CarRentalProject
php -S localhost:8000
```

Then open: `http://localhost:8000`

---

## 📊 Database Setup

### Import Database
```bash
mysql -u root -p < carrental.sql
```

### Or using PHP (if MySQL CLI not available)
```bash
php -r "
$dbh = new PDO('mysql:host=localhost', 'root', '');
\$sql = file_get_contents('carrental.sql');
\$dbh->exec(\$sql);
echo 'Database imported successfully!';
"
```

---

## 🔧 Database Configuration

Edit `config.php` with your credentials:
```php
define('DB_HOST','localhost');     // Change if MySQL not local
define('DB_USER','root');          // Your MySQL username
define('DB_PASS','');              // Your MySQL password (empty by default)
define('DB_NAME','carrental');     // Database name
```

---

## 🔐 Admin Login

**Default:**
- Username: `admin`
- Password: Hash (needs reset)

**Reset Admin Password to "Admin@123":**
```bash
mysql -u root -p carrental -e "UPDATE admin SET Password = MD5('Admin@123') WHERE UserName = 'admin';"
```

Or via phpMyAdmin:
1. Go to `admin` table
2. Edit the `admin` user row
3. Set Password to: `81dc9bdb52d04dc20036dbd8313ed055` (MD5 hash of "123456")

---

## 💻 Alternative: Run with Apache

1. Copy project to Apache webroot (usually `C:\xampp\htdocs\` or `C:\wamp\www\`)
2. Configure virtual host (optional)
3. Access via: `http://localhost/CarRentalProject` or custom domain

---

## ✅ Verify Installation

### Check PHP Works
```bash
php -v
```

### Test Database Connection
```bash
php -r "
try {
    \$db = new PDO('mysql:host=localhost;dbname=carrental', 'root', '');
    echo 'Database connection successful!';
} catch (Exception \$e) {
    echo 'Error: ' . \$e->getMessage();
}
"
```

### Check Required Extensions
```bash
php -m | findstr "PDO"
```

Should show: `PDO`, `pdo_mysql`

---

## 📋 User Roles

| User Type | Access | URL |
|-----------|--------|-----|
| **Admin** | Manage vehicles, bookings, users | `http://localhost:8000/admin/` |
| **Customer** | Browse, book vehicles, testimonials | `http://localhost:8000` |
| **Guest** | View listings only | `http://localhost:8000` |

---

## 📝 Test Workflow

1. **Start Server:**
   ```bash
   php -S localhost:8000
   ```

2. **Register User:**
   - Go to homepage
   - Click "Register"
   - Fill form and submit

3. **Login:**
   - Use registered email and password
   - Click "Login"

4. **Book Vehicle:**
   - Go to "Car Listing"
   - Select a car
   - Click "Book Now"
   - Fill booking details

5. **Access Admin Panel:**
   - Go to `http://localhost:8000/admin/`
   - Login with admin credentials
   - Manage vehicles, users, bookings

---

## 🚨 Common Errors & Fixes

| Error | Solution |
|-------|----------|
| `Access denied for user 'root'@'localhost'` | Update DB_PASS and DB_USER in config.php |
| `SQLSTATE[HY000]: General error: 3` | MySQL tmp folder permissions issue |
| `Class 'PDO' not found` | Enable `pdo_mysql` extension in php.ini |
| `No such file or directory: carrental.sql` | Run from project root directory |
| `Can't connect to MySQL server on 'localhost'` | Ensure MySQL service is running |

---

## 📺 Access Points

| Page | URL |
|------|-----|
| Home | `http://localhost:8000/` |
| Car Listing | `http://localhost:8000/car-listing.php` |
| Vehicle Details | `http://localhost:8000/vehical-details.php?vhid=1` |
| My Bookings | `http://localhost:8000/my-booking.php` |
| Admin Panel | `http://localhost:8000/admin/` |
| Contact Us | `http://localhost:8000/contact-us.php` |
| Search Results | `http://localhost:8000/search-carresult.php` |

---

## 🔄 Stop Server

Press `Ctrl + C` in terminal where PHP server is running.

---

## 📚 For Detailed Instructions
See: `SETUP_INSTRUCTIONS.md`

---

**Version:** 1.0  
**Last Updated:** March 19, 2026

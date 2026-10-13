# Signup Form Website through MariaDB on AWS

## 💡 Introduction

The Signup Form Website is a simple web application developed using HTML and PHP, with MySQL/MariaDB as the database. It allows users to submit their personal information through a signup form.

The form collects the user's name, email address, website URL, comment, and gender and stores them in the `users` table. When the user clicks the **Submit** button, the information is sent to the PHP backend and stored in the database.

### Objectives

- Create a user-friendly signup form.
- Collect user information through HTML input fields.
- Process form data using PHP.
- Store submitted information in a MySQL/MariaDB database.
- Display a submission success or error message.

---

## 🏗️ Architectural Diagram

The following diagram illustrates how the website works:

<img width="1456" height="819" alt="image" src="https://github.com/user-attachments/assets/d7e5a111-c7f9-4f6f-8905-1cccfc8c8b45" />
User/Browser → Nginx Web Server → PHP-FPM (submit.php) → MariaDB (FCT database, users table) → Success Page

### Architecture Components

| Layer | Technology | Role |
|-------|------------|------|
| Frontend | HTML | Displays the signup form and collects user information |
| Backend | PHP | Receives the submitted data, connects to MySQL, and executes the database query |
| Database | MySQL / MariaDB | Stores the submitted information in the `users` table |
| Communication | HTTP POST | Transfers the form data from the browser to the PHP backend |

---

## 📥 Input

The user's data is stored in the database `FCT`, in the table `users`:

<img width="1366" height="656" alt="image" src="https://github.com/user-attachments/assets/6812da80-d47c-4e45-8cb0-7dd2870d7357" />


The signup form collects the following information:

| Field | Description | Input Type |
|-------|-------------|------------|
| Name | User's full name | Text |
| Email | User's email address | Email |
| Website | User's website URL | URL |
| Comment | Additional information | Textarea |
| Gender | User's selected gender | Radio Button |

### Input Process

1. The user opens `signup.html` in a web browser.
2. The user enters their name and email address.
3. The user optionally enters a website URL and comment.
4. The user selects their gender.
5. The user clicks the **Submit** button.
6. The form sends the information to `submit.php` using the HTTP POST method.

### Input Validation

- Name and email are required fields.
- Gender selection is required.
- The email field checks for a valid email format in the browser.
- The website field checks for a valid URL format when a value is entered.

<img width="1911" height="1073" alt="Screenshot 2026-10-08 181716" src="https://github.com/user-attachments/assets/a7410af1-b705-4015-a287-be83b8d72710" />



> **Note:** These validations are performed by the browser. Server-side validation should also be implemented in PHP for better security.

---

## 📤 Output

After the user submits the form, the PHP script processes the information and attempts to store it in the database.

### Successful Output

If the database query succeeds, the website displays:

- A success message: `New record created successfully!`
- The submitted name
- The submitted email address
- The submitted website URL
- The submitted comment
- The selected gender




The same record is stored in the MariaDB `users` table.

### Error Output

If the database connection or insertion fails, the application displays an error message.

The submitted information is displayed on the result page using PHP's `htmlspecialchars()` function to help prevent HTML injection when rendering these values.

### Data Store Verification

Verify that the submitted data has been successfully stored in the MariaDB `users` table.

<img width="1917" height="301" alt="Screenshot 2026-10-08 181918" src="https://github.com/user-attachments/assets/14bc005d-b9bb-4332-82c5-200a6ebe8179" />


Steps to verify:

```sql
-- open MariaDB
mysql -u root -p

USE FCT;
SELECT * FROM users;
```

---

## 🚀 Deployment Guide

### 1. Launch an EC2 instance on AWS

Launch an EC2 instance and make sure the security group allows inbound HTTP (port 80) and SSH (port 22).

### 2. Push the code to GitHub

Push the project files (`signup.html`, `submit.php`, etc.) to a GitHub repository.

### 3. Install packages

```bash
sudo yum install nginx mariadb105-server php git -y
```

Start and enable the services:

```bash
sudo systemctl start nginx mariadb
sudo systemctl enable nginx mariadb
sudo systemctl status nginx mariadb
```

> PHP with Nginx also requires PHP-FPM (`sudo yum install php-fpm -y`, then start and enable `php-fpm`) and an Nginx configuration that passes `.php` files to it.

### 4. Copy the repository from GitHub

```bash
sudo git clone <repository-url>
```

### 5. Create the database to store data

```bash
mysql -u root -p
```

```sql
CREATE DATABASE FCT;
USE FCT;

CREATE TABLE users (
  id INT(11) PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50),
  email VARCHAR(100),
  website VARCHAR(200),
  comment VARCHAR(500),
  gender VARCHAR(50)
);
```

### 6. Open the application

Open the following URL in your browser:

```
http://<Public-IP>/<GitHub-repo>/signup.html
```

---

## 🏁 Conclusion

The Signup Form Website demonstrates the basic working of a web application using HTML, PHP, and MySQL. It shows how users can enter information through a frontend form, send the data to a backend script, save it in a database, and receive a result message.

This project provides practical experience with HTML forms, HTTP POST requests, PHP scripting, MySQL database connectivity, and basic web application architecture.

### Technologies Used

- **Frontend:** HTML
- **Backend:** PHP
- **Database:** MySQL / MariaDB
- **Communication:** HTTP POST
- **Optional deployment web server:** Apache or Nginx

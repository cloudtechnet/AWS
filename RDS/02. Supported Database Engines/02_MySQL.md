## ✅ **Module 2: Supported Database Engines → MySQL**

---

### 🔷 **What is MySQL?**

**MySQL** is a popular **open-source relational database management system (RDBMS)** that uses **SQL (Structured Query Language)** to manage data. It is widely used in web and enterprise applications due to its reliability, ease of use, and performance.

---

### 🔷 **Why Use MySQL in AWS RDS?**

* Fully managed DB with no need to install or patch manually
* Built-in backups, monitoring, and high availability
* Easy to scale vertically (larger instance) or horizontally (read replicas)
* Works well with **LAMP stack (Linux, Apache, MySQL, PHP/Python)**

---

### 📘 **Key Features of MySQL on RDS**

| Feature                         | Description                                        |
| ------------------------------- | -------------------------------------------------- |
| **Automated Backups**           | Daily snapshots and point-in-time recovery         |
| **Multi-AZ Deployment**         | High availability and failover support             |
| **Read Replicas**               | Supports up to 5 read replicas                     |
| **Monitoring**                  | Integrated with CloudWatch, Enhanced Monitoring    |
| **Encryption**                  | At-rest using KMS, in-transit using SSL            |
| **Parameter and Option Groups** | Customize DB behavior                              |
| **Scaling**                     | Vertical (resize instance) or horizontal (replica) |

---

## 💼 **Real-Time Example:**

> **Scenario**: A startup wants to build a blog platform using WordPress. To store articles, user data, and comments, they choose **RDS MySQL** as the backend database for scalability, automated backups, and ease of management.

---

## 🧪 **Step-by-Step Lab: Launch RDS MySQL Database**

---

### ✅ **Pre-requisites**

* AWS account (Free Tier)
* IAM user with RDS access
* EC2 instance (optional for testing DB connection)
* MySQL Workbench or terminal access

---

### 🔧 **Step 1: Go to RDS Dashboard**

* Navigate to: `AWS Console → RDS → Databases`
* Click **“Create database”**

---

### 🔧 **Step 2: Select Database Engine**

* Choose: **Standard Create**
* Engine: **MySQL**
* Version: Choose default or latest MySQL (e.g., 8.0.35)

---

### 🔧 **Step 3: Configure Settings**

* DB Instance Identifier: `mysql-lab-db`
* Master Username: `admin`
* Password: `MySecurePass123!`

---

### 🔧 **Step 4: Instance Size and Storage**

* DB Instance Class: `db.t3.micro` (Free tier)
* Storage: General Purpose (SSD) 20 GB
* Enable storage autoscaling: ✅ Optional

---

### 🔧 **Step 5: Connectivity Settings**

* VPC: Default or custom
* Subnet group: Select default
* Public access: **Yes** (for testing from local)
* VPC Security Group: Allow **port 3306** (MySQL)

---

### 🔧 **Step 6: Additional Config**

* Backup: Retention – 7 days
* Monitoring: Enable Enhanced Monitoring (Optional)
* Maintenance: Auto minor version upgrades ✅

---

### 🔧 **Step 7: Create Database**

* Click **Create database**
* Wait for **Status: Available**

---

## 🔌 **Step 8: Connect to the MySQL DB**

You can connect using:

### Option A: **MySQL Workbench**

1. Hostname: `<RDS endpoint>`
2. Port: `3306`
3. Username: `admin`
4. Password: `MySecurePass123!`
5. Click **Test Connection** → **Success**

### Option B: **Linux EC2 Instance**

```bash
sudo apt update
sudo apt install mysql-client -y

mysql -h <rds-endpoint> -u admin -p
```

---

## 💻 **Step 9: Run SQL Queries**

After login, run the following SQL commands:

```sql
CREATE DATABASE blog;
USE blog;

CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255),
  content TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO posts (title, content) 
VALUES ('First Post', 'This is my first blog post on AWS RDS!');

SELECT * FROM posts;
```

---

### 📤 **Expected Output:**

```text
+----+-----------+-----------------------------------------+---------------------+
| id | title     | content                                 | created_at          |
+----+-----------+-----------------------------------------+---------------------+
| 1  | First Post| This is my first blog post on AWS RDS!  | 2025-07-08 12:34:22 |
+----+-----------+-----------------------------------------+---------------------+
```


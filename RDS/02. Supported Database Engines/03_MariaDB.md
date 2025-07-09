## ✅ **Module 2: Supported Database Engines → MariaDB**

---

### ✅ **What is MariaDB?**

**MariaDB** is a **community-developed, open-source relational database** system, designed as a drop-in replacement for **MySQL**. It was created by the original developers of MySQL after Oracle acquired MySQL.

In **AWS RDS**, MariaDB is a **fully managed DB engine** that offers similar capabilities to MySQL with added community features and optimizations.

---

## 🔍 **Why Use MariaDB on AWS RDS?**

| Advantage                     | Description                                             |
| ----------------------------- | ------------------------------------------------------- |
| **MySQL Compatibility**       | Uses same commands, tools, and libraries                |
| **Open Source**               | No licensing cost, enterprise features available        |
| **Fully Managed by AWS**      | Backups, patching, failover, replication                |
| **Community Features**        | ColumnStore, Aria storage engine, better JSON functions |
| **Performance & Scalability** | Great for large read-heavy and mixed workloads          |

---

## 💼 **Common Use Cases**

* WordPress, Joomla, Drupal CMS applications
* Inventory management systems
* Student or employee data portals
* SaaS applications for education, healthcare

---

## 🧠 **MariaDB Architecture in AWS RDS**

1. **Primary DB Instance**: For read/write operations
2. **Read Replicas (optional)**: Scale read workloads
3. **Multi-AZ (optional)**: High availability with failover
4. **CloudWatch & Monitoring**: For performance and health
5. **IAM & KMS**: For security and access control

---

## 🧪 **Real-Time Use Case + Step-by-Step Lab**

---

### 🎯 **Use Case Scenario**:

> A college wants to build a **student registration portal**. It needs a reliable backend database to store student information, courses, and attendance. They choose **AWS RDS MariaDB** for its cost-effectiveness and MySQL compatibility.

---

## 🔧 **Step-by-Step Lab to Launch RDS MariaDB**

---

### ✅ **Lab Requirements:**

* AWS account
* IAM user with RDS permissions
* MySQL Workbench or EC2 with MySQL client

---

### 🔷 **Step 1: Go to AWS RDS Console**

* Navigate to: `AWS Console → RDS → Databases`
* Click on **“Create database”**

---

### 🔷 **Step 2: Select Database Engine**

* Choose: **Standard Create**
* Engine: **MariaDB**
* Version: e.g., **10.6.14-MariaDB**

---

### 🔷 **Step 3: DB Instance Settings**

* DB instance identifier: `mariadb-lab-db`
* Master username: `admin`
* Password: `MariaDBPass123!`

---

### 🔷 **Step 4: Instance and Storage**

* DB instance class: `db.t3.micro` (Free Tier)
* Storage: 20 GB General Purpose (SSD)
* Enable auto-scaling ✅

---

### 🔷 **Step 5: Connectivity**

* VPC: Default or Custom
* Public Access: **Yes** (for testing)
* Subnet Group: Default
* VPC Security Group: Allow **port 3306**

---

### 🔷 **Step 6: Additional Configurations**

* Enable automated backups: 7 days
* Enable deletion protection: Optional
* Enable monitoring and performance insights: Optional

---

### 🔷 **Step 7: Create Database**

* Click **Create Database**
* Wait until **Status: Available**

---

## 🔌 **Step 8: Connect to MariaDB**

### Option A: Using **MySQL Workbench**

* Host: `<your-mariadb-endpoint>`
* Port: `3306`
* Username: `admin`
* Password: `MariaDBPass123!`

Click **Test Connection** → ✅ **Success**

### Option B: Using **EC2 Linux Instance**

```bash
sudo apt update
sudo apt install mariadb-client -y

mysql -h <rds-endpoint> -u admin -p
```

---

## 💻 **Step 9: Create a Student Database**

```sql
CREATE DATABASE college;
USE college;

CREATE TABLE students (
  student_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  course VARCHAR(50),
  registered_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO students (name, email, course) 
VALUES 
('Amit Sharma', 'amit@example.com', 'Computer Science'),
('Neha Reddy', 'neha@example.com', 'Data Science');

SELECT * FROM students;
```

---

### 📤 **Expected Output:**

```text
+------------+-------------+-------------------+-------------------+---------------------+
| student_id | name        | email             | course            | registered_at       |
+------------+-------------+-------------------+-------------------+---------------------+
| 1          | Amit Sharma | amit@example.com  | Computer Science  | 2025-07-08 13:42:01 |
| 2          | Neha Reddy  | neha@example.com  | Data Science      | 2025-07-08 13:43:15 |
+------------+-------------+-------------------+-------------------+---------------------+
```


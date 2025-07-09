## ✅ **2. Supported Database Engines: Amazon Aurora**

---

### 🔹 **What is Amazon Aurora?**

Amazon **Aurora** is a **high-performance, fully managed relational database engine** developed by AWS that is **compatible with MySQL and PostgreSQL**.

It's part of the RDS family but offers:

* **5x faster performance than MySQL**
* **3x faster than PostgreSQL**
* **Built-in fault tolerance and self-healing storage**

---

### 🔹 **Aurora is Ideal for:**

* High-performance OLTP applications
* SaaS and enterprise-grade apps
* Analytics and dashboards requiring low-latency DB access

---

### 🔹 **Aurora Engine Types:**

| Aurora Variant    | Compatible With                   |
| ----------------- | --------------------------------- |
| Aurora MySQL      | MySQL 5.6, 5.7, 8.0               |
| Aurora PostgreSQL | PostgreSQL 10, 11, 12, 13, 14, 15 |

---

### 🔹 **Key Features of Amazon Aurora**

| Feature                    | Description                                       |
| -------------------------- | ------------------------------------------------- |
| **Aurora Storage**         | Auto-scales from 10 GB up to 128 TB               |
| **Aurora Replicas**        | Up to 15 read replicas                            |
| **Aurora Global Database** | Cross-region DB replication for disaster recovery |
| **Aurora Serverless v2**   | On-demand DB capacity without provisioning        |
| **Fault-tolerant**         | 6-way replication across 3 AZs                    |
| **Auto Backup & Restore**  | Point-in-time recovery, snapshot restore          |
| **Encryption**             | In-transit and at-rest via AWS KMS                |

---

## 💡 **Real-Time Use Case:**

> **Scenario**: A financial tech startup is launching a personal budgeting app. They want a reliable, highly available, and cost-effective database for handling transactions and user data. They choose **Amazon Aurora MySQL** with Multi-AZ and Read Replicas.

---

## 🧪 **Hands-On Lab: Launch Amazon Aurora MySQL DB Cluster**

### 🧾 **Pre-requisites:**

* AWS Free Tier account
* IAM user with RDS permissions
* VPC with private and public subnets
* Security Group allowing MySQL (port 3306)

---

### 🔧 **Step-by-Step Lab Instructions**

#### **Step 1: Open RDS Console**

* Go to **AWS Console → RDS → Databases**
* Click **“Create database”**

#### **Step 2: Select Engine**

* Choose **Amazon Aurora**
* Select **Aurora MySQL-compatible**
* Choose **Amazon Aurora (MySQL 8.0-compatible)**

#### **Step 3: Database Settings**

* DB Cluster Identifier: `aurora-lab-cluster`
* Master Username: `admin`
* Master Password: `Aurora123!`

#### **Step 4: Instance Configuration**

* DB Instance Class: `db.t3.small`
* Number of instances: 1 (for lab)
* Storage: Auto scaling enabled

#### **Step 5: Connectivity**

* VPC: Select your default or custom VPC
* Subnet group: Choose/create one with 2+ subnets
* Public Access: **Yes** (for testing)
* VPC Security Group: Allow inbound 3306 (MySQL)

#### **Step 6: Additional Settings**

* Enable backup (7 days)
* Enable enhanced monitoring: Optional
* Enable Performance Insights: Optional

#### **Step 7: Launch**

* Click **Create database**
* Wait 5–10 minutes until **Status: Available**

---

## 🔌 **Step 8: Connect to Aurora MySQL DB**

### Option A: From EC2 Instance (preferred)

1. Launch an EC2 instance in the **same VPC**
2. Install MySQL client:

   ```bash
   sudo apt update
   sudo apt install mysql-client -y
   ```
3. Connect to DB:

   ```bash
   mysql -h <aurora-endpoint> -u admin -p
   ```

### Option B: From Local Machine (with public access enabled)

1. Use MySQL Workbench or terminal:

   ```bash
   mysql -h <aurora-endpoint> -u admin -p
   ```

---

## 🔁 **Step 9: Test Query**

After logging in:

```sql
CREATE DATABASE myapp;
USE myapp;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO users (name, email) VALUES ('Rajesh', 'rajesh@example.com');
SELECT * FROM users;
```

---

## 📤 **Expected Output:**

```text
+----+--------+--------------------+---------------------+
| id | name   | email              | created_at          |
+----+--------+--------------------+---------------------+
| 1  | Rajesh | rajesh@example.com | 2025-07-08 09:01:22 |
+----+--------+--------------------+---------------------+
```

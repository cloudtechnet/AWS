## 🧠 Module: 5. RDS Features

### 📌 Topic: **Performance Insights**

---

### 🔍 What is Performance Insights?

**Performance Insights** is an Amazon RDS feature that helps you **monitor and analyze the database load** so you can quickly detect and resolve performance issues. It provides **real-time** and **historical performance metrics**, making it easier to identify:

* Top SQL queries
* Wait events (what the DB is waiting on)
* High-load timeframes
* Resource bottlenecks (like CPU or I/O)

> 🧠 Think of it as the “Task Manager” for your RDS database.

---

### 💡 Key Benefits

| Feature                     | Benefit                                         |
| --------------------------- | ----------------------------------------------- |
| DB Load Visualization       | Visualize load by average active sessions (AAS) |
| Query-level Analysis        | Identify expensive SQL queries                  |
| Integration with CloudWatch | Easy monitoring and alerting                    |
| Retention                   | 7-day free history (can upgrade to 2 years)     |

---

### 🏥 Real-time Use Case:

**Project Scenario:**

> You are managing an e-commerce website on an RDS MySQL instance. During Black Friday, users complain about slow website response. You need to investigate what's causing high DB load.

**Solution:** Use Performance Insights to track **top queries**, **waits**, and **active sessions** and optimize the query/connection pool.

---

### 🧪 Step-by-Step Lab: Enable & Use Performance Insights

---

#### 🔧 Prerequisites:

* AWS account
* IAM role with RDS Full Access
* Existing or new RDS DB instance (MySQL/PostgreSQL/Aurora)

---

### 🧭 Step 1: Launch RDS with Performance Insights Enabled

1. Go to **AWS Console > RDS > Databases**.
2. Click on **“Create database”**.
3. Choose:

   * Engine: **MySQL**
   * DB instance class: `db.t3.medium` (or higher)
4. Scroll to **“Monitoring”** section.
5. ✅ **Enable Performance Insights**
6. Choose retention period: (default is 7 days)
7. ✅ Create a **new KMS key** or use default if asked.
8. Leave other settings default and click **Create Database**.

---

### 🧭 Step 2: Simulate Load (Optional for Lab)

You can use a small script or SQL load generator to simulate multiple connections and queries.

Example:

```sql
DELIMITER //
CREATE PROCEDURE insert_dummy_data()
BEGIN
  DECLARE i INT DEFAULT 0;
  WHILE i < 10000 DO
    INSERT INTO test_table (name, created_at) VALUES (CONCAT('User', i), NOW());
    SET i = i + 1;
  END WHILE;
END;
//
DELIMITER ;

CALL insert_dummy_data();
```

---

### 🧭 Step 3: View Performance Insights

1. Go to **RDS Console > Databases > your-db-instance**
2. Scroll down and click on **“Performance Insights”**
3. Visual chart will show:

   * **Average Active Sessions (AAS)**
   * **Top SQL statements**
   * **Top waits** (like `cpu`, `io`, `lock`)
4. Use the dropdown to change **time range** and zoom into problem windows.
5. Click on the **Top SQL ID** to view query text and performance breakdown.

---

### 🧭 Step 4: Analyze and Take Action

* If a specific query is consuming most load → Optimize it (add index, tune logic).
* If CPU is maxed out → Consider resizing instance or improving caching.
* If IO waits are high → Review storage performance or connection pool settings.

---

### 📌 Optional: Disable/Modify Performance Insights

1. Go to **Modify DB Instance**
2. Scroll to **Monitoring**
3. ✅ Uncheck **Enable Performance Insights** or change KMS key/retention
4. Click **Continue > Apply Immediately**

---

### 🎓 Teaching Tips

* Show **live graph change** after query load.
* Ask students to create a **slow SQL** and view its impact.
* Discuss **how to interpret wait events** (like `cpu`, `io`, `buffer latch`, etc.)

---

### 📘 Summary

| Feature      | Description                                          |
| ------------ | ---------------------------------------------------- |
| Type         | Visual performance diagnostics                       |
| Use Case     | Troubleshoot slow queries and bottlenecks            |
| Availability | Most engines (MySQL, PostgreSQL, Aurora, SQL Server) |
| Lab Output   | Graphs showing DB load, slow SQL, waits              |


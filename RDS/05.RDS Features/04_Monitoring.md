## **Module 5: RDS Features**

### **Topic: Monitoring (CloudWatch, Enhanced Monitoring)**

---

### **Objective:**

To explain how AWS RDS monitoring works using **CloudWatch** and **Enhanced Monitoring**, why it is important, and how to set it up with a **real-time lab example**.

---

## **🧠 What is Monitoring in RDS?**

Monitoring is a critical feature of RDS to help administrators:

* Track **performance metrics** (CPU, memory, disk, etc.)
* Set **threshold alarms**
* **Troubleshoot slow queries or degraded performance**
* **Audit usage patterns** over time

AWS provides two monitoring options:

---

### ✅ **1. Amazon CloudWatch (Basic Monitoring)**

* Collects **metrics every 1 minute or 5 minutes**.
* Default and free-tier eligible.
* Basic metrics include:

  * CPUUtilization
  * FreeStorageSpace
  * DatabaseConnections
  * Read/Write IOPS
  * NetworkThroughput

---

### ✅ **2. Enhanced Monitoring**

* Real-time **OS-level metrics** (every 1, 5, 10, 15, 30, or 60 seconds).
* Includes:

  * **RAM usage**
  * **Disk I/O per process**
  * **CPU breakdown by core**
  * **Swap usage**
* Requires **IAM role** to allow RDS to push data to CloudWatch Logs.

---

## **📌 Real-Time Use Case Example:**

> "You are running a production MySQL RDS instance and the application is experiencing latency. You want to monitor OS-level metrics to identify bottlenecks (RAM, CPU, or IOPS). You decide to enable Enhanced Monitoring."

---

## 🔬 **Lab Example: Monitor RDS using CloudWatch & Enhanced Monitoring**

---

### **Step 1: Launch RDS Instance**

1. Go to **RDS Console → Databases → Create Database**
2. Engine: **MySQL**
3. DB Instance Identifier: `monitoring-demo`
4. DB Instance Class: `db.t3.micro`
5. Storage: 20 GB (default)
6. Enable:

   * **Public Access: No**
   * VPC Security Group allowing access from your IP
7. Database authentication: Username/password

🟢 *Click "Create Database"*

---

### **Step 2: View CloudWatch Basic Metrics**

1. Go to **CloudWatch Console**
2. Navigate to **Metrics → RDS**
3. Click on your DB identifier `monitoring-demo`
4. Observe:

   * CPUUtilization
   * FreeableMemory
   * ReadLatency
   * WriteLatency
   * DatabaseConnections

📝 *These metrics update every 1–5 minutes.*

---

### **Step 3: Enable Enhanced Monitoring**

1. Go to **RDS Console → Databases → `monitoring-demo`**
2. Click **Modify**
3. Scroll to **Monitoring**
4. Set:

   * **Enable Enhanced Monitoring: Yes**
   * **Monitoring Role: rds-monitoring-role (Create if not exists)**
   * **Granularity: 1 second**
5. Click **Continue → Apply Immediately**

---

### **Step 4: View Enhanced Monitoring Metrics**

1. Go back to RDS → `monitoring-demo` → **Monitoring Tab**
2. Scroll down to view:

   * CPU Usage per core
   * RAM utilization
   * Disk I/O
   * Swap usage

These metrics are **real-time** and more granular than CloudWatch.

---

### **Step 5: Create CloudWatch Alarm**

1. Go to **CloudWatch → Alarms → Create Alarm**
2. Select metric:

   * RDS → Per-Database Metrics → CPUUtilization
3. Set:

   * Threshold: **> 70%**
   * Period: 1 minute
4. Action:

   * Send email using **SNS topic**
   * Confirm subscription via email

🟢 *This notifies you when CPU is high.*

---

## 🧾 Summary to Students:

| Feature         | CloudWatch     | Enhanced Monitoring |
| --------------- | -------------- | ------------------- |
| Level           | Instance-level | OS-level            |
| Frequency       | 1 or 5 minutes | 1 to 60 seconds     |
| Real-time?      | No             | Yes                 |
| IAM Role needed | No             | Yes                 |

---

## 📚 When to Use:

* **CloudWatch** for high-level health checks, threshold-based alerts.
* **Enhanced Monitoring** when:

  * You troubleshoot **high latency**
  * Need **OS-level visibility**
  * Monitor **multi-core CPU performance**

# ✅ **Module 1: Introduction to VPC**

---

## 🔹 **What is Amazon VPC?**

### ✅ Definition:

Amazon VPC (Virtual Private Cloud) is a logically isolated **virtual network** within the AWS cloud where you can **launch AWS resources** such as EC2 instances, RDS databases, etc., with **full control over networking configuration**.

### ✅ Key Features:

* **Custom IP Address Ranges** using CIDR blocks (e.g., 10.0.0.0/16)
* **Subnet creation** to isolate resources (public & private)
* **Routing control** via route tables
* **Security** through Security Groups and Network ACLs
* Support for **VPNs and Direct Connect** for hybrid environments

### ✅ Real-Time Analogy:

Think of a VPC like a **virtual data center** in the cloud where you design your network infrastructure exactly how you want.

### ✅ Use Cases:

* Hosting a public website with a backend database
* Creating a hybrid cloud setup with on-prem connectivity
* Building isolated environments for dev, test, and prod

---

## 🔹 **Why Use a VPC?**

### ✅ 1. **Isolation & Security**

* VPC is **logically isolated**, meaning your network is private within AWS.
* You can control **who accesses what** using **Security Groups, NACLs, and IAM**.

### ✅ 2. **Full Control over Network Configuration**

* Choose your own IP ranges
* Define custom route tables
* Set up **internet/NAT gateways** based on your needs

### ✅ 3. **Granular Access Control**

* Secure traffic using **firewalls, flow logs**
* Segment networks using **subnets and NACLs**

### ✅ 4. **Scalability and Integration**

* Easily scale by adding subnets or peering with other VPCs
* Connect to other AWS services securely (S3, DynamoDB via VPC endpoints)

### ✅ 5. **Compliance & Audit**

* Helps meet compliance like PCI-DSS, HIPAA by controlling data flow
* Use VPC Flow Logs for **auditing and troubleshooting**

---

## 🔹 **Default VPC vs Custom VPC**

| Feature                | Default VPC                                  | Custom VPC                    |
| ---------------------- | -------------------------------------------- | ----------------------------- |
| Created Automatically? | Yes (in each region)                         | No (user must create it)      |
| Subnets                | 1 subnet per AZ (by default)                 | You define subnets as needed  |
| Internet Gateway       | Automatically attached                       | Must manually attach          |
| Route Table            | Default routes to Internet                   | Custom routes needed          |
| Use Case               | Quick testing or prototyping                 | Production-grade architecture |
| Security Groups        | Default SG allows all internal communication | Must be defined by user       |

### ✅ **Default VPC**

* Comes pre-configured in every AWS region
* Great for **beginners or test environments**
* Has **public subnets and a working route to internet**

### ✅ **Custom VPC**

* Built from scratch as per the organization's **architecture, security, and compliance**
* Provides **complete control** over networking, making it ideal for **production use**

### 🧠 Tip for Students:

- Default VPC = Quick start for learning
- Custom VPC = Must-have for real-world cloud solutions

---



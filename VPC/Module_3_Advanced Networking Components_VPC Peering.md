# ✅ **Module 3: Advanced Networking Components**

## 🔷 **Topic: VPC Peering**

---

## 🔹 What is VPC Peering?

### ✅ Definition:

**VPC Peering** is a networking connection between two VPCs that enables you to **route traffic between them privately**, using **private IP addresses**, without using internet, VPN, or Direct Connect.

It allows instances in both VPCs to communicate **as if they are in the same network**.

---

### ✅ Key Characteristics:

* Peering is **one-to-one** (not transitive)
* Works **across accounts and regions** (with some conditions)
* No bandwidth bottleneck (uses AWS backbone)
* Must update **route tables and security groups** to allow traffic

---

### ✅ Use Cases:

* You have **VPC A** running web servers and **VPC B** running database servers.
* Your **Dev** and **Prod** environments are in **separate VPCs**, but need communication.
* You need **cross-region private communication** between apps in different VPCs.

---

## 🔷 Real-Time Use Case Scenario (Lab)

### 🧾 Objective:

Establish VPC Peering between two VPCs (`vpc-app` and `vpc-db`) in the same region so that an EC2 instance in one VPC can **ping and communicate** with an EC2 instance in the other VPC via **private IP**.

---

## 🧪 **Step-by-Step Lab Guide**

### ✅ Pre-Requisites:

* Two VPCs in the same AWS region:

  * `vpc-app`: 10.0.0.0/16
  * `vpc-db`: 10.1.0.0/16
* One subnet and one EC2 instance in each VPC
* EC2 instances should be Amazon Linux 2
* Allow ICMP (ping) in **Security Group**

---

### 🔁 Step 1: Create 2 VPCs

1. **VPC A - App VPC**

   * CIDR: `10.0.0.0/16`
2. **VPC B - DB VPC**

   * CIDR: `10.1.0.0/16`

---

### 🖥️ Step 2: Launch EC2 Instances in Each VPC

* EC2-A in `vpc-app` (e.g., 10.0.1.10)
* EC2-B in `vpc-db` (e.g., 10.1.1.10)
* Use key pair and allow:

  * **SSH (port 22)**
  * **ICMP - Echo Request**

---

### 🔁 Step 3: Create VPC Peering Connection

1. Go to **VPC Console > Peering Connections > Create Peering Connection**
2. Name: `app-db-peering`
3. Requester VPC: `vpc-app`
4. Accepter VPC: `vpc-db` (same account)
5. Click **Create Peering Connection**
6. Click on the newly created peering > **Actions > Accept Request**

---

### 📘 Step 4: Update Route Tables

1. **For VPC A Route Table** (app):

   * Destination: `10.1.0.0/16` → Target: `pcx-xxxxxxx`
2. **For VPC B Route Table** (db):

   * Destination: `10.0.0.0/16` → Target: `pcx-xxxxxxx`

> 💡 Without this, packets won’t route between VPCs.

---

### 🔐 Step 5: Update Security Groups

For both EC2s, **add inbound rule**:

* Type: **All ICMP - IPv4**
* Source: Custom IP `10.0.0.0/8` or specific other VPC CIDR

---

### 🧪 Step 6: Test Connectivity

1. SSH into EC2-A:

   ```bash
   ssh -i mykey.pem ec2-user@<Public IP of EC2-A>
   ```
2. Ping EC2-B private IP:

   ```bash
   ping 10.1.1.10
   ```

---

### ✅ **Expected Output:**

```bash
PING 10.1.1.10 (10.1.1.10) 56(84) bytes of data.
64 bytes from 10.1.1.10: icmp_seq=1 ttl=254 time=1.20 ms
64 bytes from 10.1.1.10: icmp_seq=2 ttl=254 time=1.25 ms
```

---

### ❌ Common Errors & Fixes:

| Error                  | Fix                                |
| ---------------------- | ---------------------------------- |
| Ping fails             | Check route table entries and SGs  |
| SSH works, but no ping | ICMP not allowed in SG             |
| Timeout                | Peering not accepted or wrong CIDR |

---

## 📘 Teaching Summary:

| Component          | Description                       |
| ------------------ | --------------------------------- |
| Peering Connection | Private tunnel between 2 VPCs     |
| Route Table Update | Ensures traffic knows where to go |
| Security Groups    | Controls access to/from instances |
| Ping Output        | Proves private connectivity works |

---

## 📊 Slide/Visual Suggestions:

* Use VPC Diagram:

  ```
  [VPC-A] → Peering ↔ [VPC-B]
      |                    |
   EC2-A               EC2-B
  ```
* Add screenshots for:

  * Peering creation
  * Route table entries
  * Ping test result



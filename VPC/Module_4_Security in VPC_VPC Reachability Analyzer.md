# ✅ Module 4: Security in VPC

## 🔷 Topic: **VPC Reachability Analyzer**

---

## 🔹 What is VPC Reachability Analyzer?

### ✅ Definition:

**VPC Reachability Analyzer** is an AWS network troubleshooting tool that **analyzes connectivity** between two resources in your VPC — such as **EC2 instances, ENIs, Load Balancers, NAT Gateways, or VPC endpoints** — and tells you **whether network traffic can reach the destination**, and if not, **why it fails**.

---

## ✅ Why Use Reachability Analyzer?

| Use Case              | Purpose                                                   |
| --------------------- | --------------------------------------------------------- |
| ✅ Troubleshooting     | Identify **why traffic is not reaching** a resource       |
| ✅ Security validation | Ensure **NACL, SG, Route Table** are not blocking traffic |
| ✅ Planning            | Simulate network behavior before deployment               |
| ✅ Documentation       | Visual explanation of path for audits and teams           |

---

## 🧠 Key Concepts

| Term                  | Description                                                          |
| --------------------- | -------------------------------------------------------------------- |
| **Source**            | The starting point of the analysis (ENI, EC2, ALB, etc.)             |
| **Destination**       | The target resource (ENI, ALB, EC2, Gateway, etc.)                   |
| **Path Status**       | Whether the path is **Reachable** or **Unreachable**                 |
| **Hop**               | A step in the traffic path (e.g., route table, security group, NACL) |
| **Blocked Component** | The exact resource (like SG/NACL) that is blocking the path          |

---

# 🧪 Real-Time Lab: Analyze EC2 to EC2 Reachability

---

## 🎯 Objective:

You have **two EC2 instances** in different **subnets** or **AZs** and want to:

* Check if traffic (like ping/SSH) is allowed
* Use VPC Reachability Analyzer to confirm connectivity

---

## 🔹 Lab Architecture

```
               VPC: 10.0.0.0/16
              ┌──────────────┐
              │              │
   Subnet A (10.0.1.0/24)    Subnet B (10.0.2.0/24)
   ┌────────────┐            ┌────────────┐
   │ EC2-1      │────────────► EC2-2      │
   │ (10.0.1.10)│     Ping/SSH?           │
   └────────────┘            └────────────┘
```

---

## ✅ Step 1: Create VPC and Two Subnets

1. Create a **VPC** with CIDR `10.0.0.0/16`
2. Create **two public subnets**:

   * Subnet A: `10.0.1.0/24`
   * Subnet B: `10.0.2.0/24`
3. Associate the subnets with a **public route table** (with IGW attached)

---

## ✅ Step 2: Launch EC2 Instances

* **EC2-1** in Subnet A
* **EC2-2** in Subnet B
* Use Amazon Linux 2 AMI for both
* Assign public IPs
* Use same key pair for SSH

---

## ✅ Step 3: Configure Security Groups

For **EC2-2** (Destination):

* Allow **ICMP (ping)** and **TCP port 22 (SSH)** **from EC2-1’s IP**
  Example:

```text
Inbound Rule:
- Type: ICMP - Echo Request, Source: 10.0.1.10/32
- Type: SSH, Source: 10.0.1.10/32
```

For **EC2-1**:

* Outbound: default (allow all)

---

## ✅ Step 4: Optional — Create NACL Deny Rule (to simulate failure)

If you want to **block communication**, add to NACL of Subnet B:

```text
Inbound Rule:
- Rule #100: Deny ICMP from 10.0.1.0/24
```

---

## ✅ Step 5: Test Ping/SSH (Manual)

From **EC2-1**:

```bash
ping 10.0.2.10
ssh ec2-user@10.0.2.10
```

If blocked → it will timeout
If allowed → you’ll get response or login

---

## ✅ Step 6: Open VPC Reachability Analyzer

1. Go to **VPC Console → Reachability Analyzer**
2. Click **Create and Run Analysis**

### Fill the form:

| Field           | Value                      |
| --------------- | -------------------------- |
| **Source**      | EC2-1’s ENI                |
| **Destination** | EC2-2’s ENI                |
| **Protocol**    | TCP or ICMP                |
| **Port**        | 22 or Leave blank for ICMP |

Click **Run Analysis**

---

## ✅ Step 7: Review Results

### 🔸 If Path is **Reachable**:

* ✅ All routing, security groups, and NACLs are correctly configured

### 🔸 If Path is **Unreachable**:

You’ll get a message like:

```
Traffic is blocked at subnet NACL inbound rule
```

Or

```
Security group does not allow inbound SSH on port 22
```

✅ Click each **Hop** in the analysis path to **visually inspect**:

* Routing decisions
* SG evaluations
* NACL blocks
* ENI reachability

---

## ✅ Step 8: Fix Any Issues

Based on feedback:

* Update SG or NACL rules
* Rerun the analyzer
* Confirm status becomes **Reachable**

---

## 🔍 Output Sample:

### Example Summary:

```
Reachability: Unreachable
Blocked Component: Subnet NACL
Hop: Inbound rule blocking ICMP from 10.0.1.0/24
```

✅ After removing the deny rule → run again → **Reachability: Reachable**

---

## 🧠 Teaching Summary Table

| Feature | Security Group | NACL                 | Reachability Analyzer      |
| ------- | -------------- | -------------------- | -------------------------- |
| Scope   | ENI            | Subnet               | Cross-VPC resource check   |
| State   | Stateful       | Stateless            | N/A (analysis tool)        |
| Purpose | Access Control | Broad subnet control | Troubleshooting & auditing |

---

## 🖼️ PPT/Whiteboard Diagram

```
              ┌─────────────────────────────┐
              │ VPC Reachability Analyzer   │
              └────────────┬────────────────┘
                           │
                Analyze path between:
                           ▼
   ┌─────────────┐                     ┌─────────────┐
   │  EC2-1      │-------------------->│  EC2-2      │
   │ Subnet A    │    TCP/ICMP         │ Subnet B    │
   └─────────────┘                     └─────────────┘
                           ▲
        Visual path via SG/NACL/Route Tables
```

---

## 📘 Real-World Use Cases

| Scenario                   | Benefit of Reachability Analyzer |
| -------------------------- | -------------------------------- |
| App can't talk to DB       | Pinpoint SG or route issues      |
| Load Balancer unreachable  | Validate path to targets         |
| Cross-AZ EC2 traffic fails | Show NACL or IGW issues          |
| Validate compliance        | Prove network path visibility    |

---

## 🧹 Cleanup

* Terminate EC2 instances
* Delete Flow Log (if created)
* Delete Reachability analysis history (optional)


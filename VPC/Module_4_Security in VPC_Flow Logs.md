# ✅ Module 4: Security in VPC

## 🔷 Topic: **Flow Logs (VPC Logging & Monitoring)**

---

## 🔹 What Are VPC Flow Logs?

### ✅ Definition:

**VPC Flow Logs** capture **IP traffic** going **to and from network interfaces** (ENIs) in your VPC and **log it** to Amazon CloudWatch Logs or Amazon S3.

---

## 🔹 Why Use VPC Flow Logs?

| Benefit                         | Description                                             |
| ------------------------------- | ------------------------------------------------------- |
| 🔎 **Monitor Traffic**          | View accepted or rejected traffic                       |
| 🔐 **Troubleshoot Security**    | Identify blocked traffic by security groups/NACLs       |
| 📜 **Audit Network Access**     | Maintain a record of IP-based communication             |
| 📊 **Analyze Traffic Patterns** | Useful for security, compliance, and performance tuning |
| 🔔 **Detect Intrusions**        | Spot suspicious access attempts                         |

---

## 🔹 Where Can You Enable Flow Logs?

You can enable flow logs at:

* VPC level (all subnets and ENIs)
* Subnet level
* ENI (Elastic Network Interface) level (EC2, NAT, LB, etc.)

---

## 🔹 What Does a Flow Log Record Contain?

Example record format:

```
version account-id interface-id srcaddr dstaddr srcport dstport protocol packets bytes start end action log-status
```

Example output:

```
2 123456789012 eni-0ab1234cdef5678gh 10.0.1.10 10.0.2.20 443 49152 6 10 840 1617711042 1617711102 ACCEPT OK
```

---

# 🧪 Real-Time Lab: Enable and Analyze VPC Flow Logs

---

## 🎯 Scenario:

You want to monitor **inbound and outbound traffic** for a **VPC** hosting an EC2 web server to:

* Confirm HTTP traffic is accepted
* Detect any denied traffic (if present)

---

## 🔹 Lab Setup Overview

| Component       | Details                           |
| --------------- | --------------------------------- |
| VPC             | `10.0.0.0/16`                     |
| Subnet          | `10.0.1.0/24` (public)            |
| EC2 Instance    | Amazon Linux 2, Apache web server |
| Flow Log Target | CloudWatch Logs                   |

---

## ✅ Step 1: Launch EC2 with Apache Web Server

1. Launch an EC2 instance in a public subnet
2. Install Apache:

```bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Hello from Flow Log Demo" | sudo tee /var/www/html/index.html
```

✅ Visit: `http://<EC2-Public-IP>`
Expected Output:

```
Hello from Flow Log Demo
```

---

## ✅ Step 2: Create CloudWatch Log Group (Optional)

1. Go to **CloudWatch → Log groups**
2. Click **Create log group**
3. Name it `VPCFlowLogsGroup`

---

## ✅ Step 3: Enable Flow Logs for VPC

1. Go to **VPC Dashboard → Your VPCs**
2. Select the VPC (e.g., `10.0.0.0/16`)
3. Click **Actions → Create Flow Log**

### Configure:

* **Filter**: `ALL` (to log both accepted and rejected traffic)
* **Destination**: `Send to CloudWatch Logs`
* **Log group**: `VPCFlowLogsGroup` (or create new)
* **IAM Role**: Choose an existing role or let AWS create one

Click **Create Flow Log**

---

## ✅ Step 4: Generate Traffic

1. From your browser:

   * Open the EC2 public IP in browser (`http://<EC2-IP>`)
2. From EC2 instance:

   * Try a `curl` to an external IP to generate outbound traffic:

```bash
curl http://example.com
```

---

## ✅ Step 5: View Flow Log Records in CloudWatch

1. Go to **CloudWatch > Log groups > VPCFlowLogsGroup**
2. Select the log stream for your ENI
3. You’ll see logs like:

```plaintext
2 1234567890 eni-0ab12345cde6789f 10.0.1.10 93.184.216.34 41542 80 6 5 2320 1697590920 1697590980 ACCEPT OK
2 1234567890 eni-0ab12345cde6789f 203.0.113.25 10.0.1.10 80 50500 6 3 1500 1697590920 1697590980 ACCEPT OK
```

Each line = one network flow (TCP connection)

---

## ✅ Step 6: Interpret the Fields

| Field      | Meaning                    |
| ---------- | -------------------------- |
| srcaddr    | Source IP                  |
| dstaddr    | Destination IP             |
| srcport    | Source port                |
| dstport    | Destination port           |
| protocol   | 6 = TCP, 17 = UDP          |
| packets    | Number of packets          |
| bytes      | Bytes transferred          |
| action     | `ACCEPT` or `REJECT`       |
| log-status | `OK`, `NODATA`, `SKIPDATA` |

---

## 🧠 Teaching Summary Table

| Use Case                   | SG/NACL | Flow Logs Help With         |
| -------------------------- | ------- | --------------------------- |
| Web access troubleshooting | ✅ SG    | Confirm port 80 is ACCEPTED |
| Blocked SSH attempt        | ✅ NACL  | View REJECTed entries       |
| Audit app connections      | -       | Check IP, ports, and usage  |
| Security analysis          | -       | Spot unusual IPs or traffic |

---

## 🖼️ PPT/Whiteboard Diagram

```
                     ┌──────────────────┐
                     │   CloudWatch     │
                     │ Log Group        │
                     └────┬─────────────┘
                          │
                     [ VPC Flow Logs ]
                          │
      ┌────────────┐      ▼       ┌────────────┐
      │ Public Subnet │         │ Flow Log Records│
      └────────────┘               └────────────┘
             │
        ┌────▼─────┐
        │  EC2     │
        └──────────┘
        Apache Web Server
```

---

## 📘 Real-Time Use Cases

| Use Case              | Description                                      |
| --------------------- | ------------------------------------------------ |
| 🔐 Security Forensics | Who accessed this instance, from where?          |
| 🧪 Troubleshooting    | Why is this port blocked or service unreachable? |
| 📊 Traffic Analysis   | How much data is flowing in/out of VPC?          |
| 🚫 Detect Threats     | Block or investigate unknown access attempts     |

---

## ✅ Cleanup (Post Lab)

* Stop or terminate EC2
* Delete CloudWatch log group (optional)
* Remove Flow Logs to avoid future billing





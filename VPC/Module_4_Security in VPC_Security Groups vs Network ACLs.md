# ✅ Module 4: Security in VPC

## 🔷 Topic: **Security Groups vs Network ACLs**

---

## 🔹 What is a Security Group (SG)?

### ✅ Definition:

A **Security Group** acts as a **virtual firewall for EC2 instances**, controlling **inbound and outbound traffic** at the **instance level**.

---

### ✅ Key Characteristics:

| Feature    | Description                                            |
| ---------- | ------------------------------------------------------ |
| Scope      | **Instance-level** (ENI attached to EC2)               |
| State      | **Stateful** – return traffic is automatically allowed |
| Rules      | Allow only (no deny)                                   |
| Default    | Denies all inbound, allows all outbound                |
| Applied To | ENIs (Elastic Network Interfaces)                      |

---

## 🔹 What is a Network ACL (NACL)?

### ✅ Definition:

A **Network ACL** is a **stateless firewall** that controls **traffic in and out of a subnet**.

---

### ✅ Key Characteristics:

| Feature    | Description                                               |
| ---------- | --------------------------------------------------------- |
| Scope      | **Subnet-level**                                          |
| State      | **Stateless** – return traffic must be explicitly allowed |
| Rules      | Allow and Deny                                            |
| Default    | Allows all inbound and outbound                           |
| Applied To | Entire subnet (all instances inside)                      |

---

## 🔁 SG vs NACL: Key Differences

| Feature         | Security Group                   | Network ACL                       |
| --------------- | -------------------------------- | --------------------------------- |
| Applied to      | EC2 instance (ENI)               | Entire Subnet                     |
| Rules           | Allow only                       | Allow and Deny                    |
| Stateful?       | ✅ Yes                            | ❌ No (Stateless)                  |
| Default Rules   | Inbound denied, Outbound allowed | Inbound and Outbound allowed      |
| Rule Evaluation | All rules evaluated              | Rule number order (lowest wins)   |
| Use Case        | Instance-level security          | Broad subnet-level access control |

---

## 🧾 Real-World Scenario:

You have a **web application** with EC2 instances in a **public subnet**.
You want to allow:

* Only HTTP (port 80) and SSH (port 22) from your IP
* Block **all traffic** from a specific IP at the **subnet level**

You will use:

* ✅ **Security Group** to allow only specific ports
* ✅ **Network ACL** to block an IP (deny rule)

---

# 🧪 Step-by-Step Lab: SG vs NACL

---

## 🔹 Lab Setup Overview:

| Component      | Value                         |
| -------------- | ----------------------------- |
| VPC            | 10.0.0.0/16                   |
| Public Subnet  | 10.0.1.0/24                   |
| EC2 Instance   | Amazon Linux 2                |
| Security Group | Custom SG with HTTP/SSH rules |
| NACL           | Block IP: `203.0.113.25`      |

---

## ✅ Step 1: Launch EC2 Instance in Public Subnet

1. Create a **VPC** with a **public subnet**
2. Launch an EC2 instance in that subnet (Amazon Linux 2)
3. Assign a **public IP**
4. SSH into the instance

---

## ✅ Step 2: Configure Apache Web Server

```bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Welcome to SG vs NACL Demo" | sudo tee /var/www/html/index.html
```

Verify:

```bash
curl http://localhost
```

✅ Output:

```
Welcome to SG vs NACL Demo
```

---

## ✅ Step 3: Configure Security Group

Edit the EC2’s SG:

* **Inbound Rules**:

  * Allow SSH (22) from your IP
  * Allow HTTP (80) from `0.0.0.0/0`

* **Outbound Rules**: Leave default (allow all)

✅ Test in browser:

```bash
http://<EC2-public-IP>
```

✅ You should see:

```
Welcome to SG vs NACL Demo
```

---

## ✅ Step 4: Create Custom NACL

1. Go to **VPC → Network ACLs**
2. Create a new NACL (name: `BlockSpecificIP`)
3. Associate it with the **public subnet**

---

## ✅ Step 5: Add NACL Rules

| Rule # | Type        | Protocol | Port | Source            | Action |
| ------ | ----------- | -------- | ---- | ----------------- | ------ |
| 100    | HTTP        | TCP      | 80   | 0.0.0.0/0         | ALLOW  |
| 110    | SSH         | TCP      | 22   | Your IP           | ALLOW  |
| 120    | ALL Traffic | ALL      | ALL  | `203.0.113.25/32` | DENY   |
| \*     | ALL Traffic | ALL      | ALL  | 0.0.0.0/0         | DENY   |

📌 Add **same rules** for **Outbound rules** (remember: NACLs are stateless)

---

## ✅ Step 6: Test Behavior

* From your IP:

  * ✅ You should be able to SSH and access the webpage

* From `203.0.113.25`: (Simulate using VPN or online proxy)

  * ❌ Access denied — blocked by NACL

---

## ✅ Step 7: Check Flow Logs (Optional)

Enable **VPC Flow Logs** to verify NACL-denied connections:

1. Go to **VPC → Your VPC → Flow Logs**
2. Enable for the **subnet**
3. Look for `REJECT` records from blocked IP

---

## 🧠 Teaching Summary Table

| Action                    | Controlled By  | Rule Type | Stateful? |
| ------------------------- | -------------- | --------- | --------- |
| Allow HTTP from all       | Security Group | ALLOW     | Yes       |
| Block IP `203.0.113.25`   | Network ACL    | DENY      | No        |
| SSH only from specific IP | Security Group | ALLOW     | Yes       |

---

## 🖼️ Suggested PPT Diagram

```
         ┌────────────────────────────┐
         │        Public Subnet       │
         │   10.0.1.0/24 (has NACL)   │
         └────────────┬───────────────┘
                      │
        ┌─────────────▼──────────────┐
        │       EC2 Instance         │
        │ Apache Web (Port 80)       │
        │ Security Group:            │
        │   - Allow 80 (0.0.0.0/0)   │
        │   - Allow 22 (your IP)     │
        └────────────────────────────┘
```

> 🔒 NACL blocks traffic from `203.0.113.25`, even though SG allows HTTP.

---

## 📘 Real-World Use Case

* Use **SGs** for day-to-day access control (e.g., web, SSH, DB)
* Use **NACLs** to block entire IPs, ranges, or malicious traffic at subnet-level



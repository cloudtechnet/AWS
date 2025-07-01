# ✅ Module 4: Security in VPC

## 🔷 Topic: **AWS Network Firewall (Advanced)**

---

## 🔹 What is AWS Network Firewall?

### ✅ Definition:

**AWS Network Firewall** is a **managed network security service** that helps you **inspect, filter, and block network traffic** at the **VPC level**, using:

* **Stateful inspection**
* **Stateless rules**
* **Domain name filtering**
* **Suricata-compatible rules**

It is deployed at the **VPC border (edge)**, protecting subnets and workloads from malicious traffic.

---

## ✅ Key Features

| Feature                       | Description                                  |
| ----------------------------- | -------------------------------------------- |
| 🔐 Deep packet inspection     | Identifies threats based on payload          |
| 🌐 Domain name filtering      | Blocks access to specific domains            |
| 🛡️ Suricata rules            | Open-source rule sets for threat detection   |
| 🚦 Stateless & Stateful rules | Flexibility in handling packet filtering     |
| 📊 Logging                    | Send flow logs to CloudWatch, S3, or Kinesis |

---

## 🔹 AWS Network Firewall Components

| Component                 | Description                                                           |
| ------------------------- | --------------------------------------------------------------------- |
| **Firewall**              | The actual enforcement point deployed in a VPC                        |
| **Firewall Policy**       | Defines stateless/stateful rules                                      |
| **Rule Groups**           | Collection of filtering rules                                         |
| **Logging Configuration** | Where to send alert/logs                                              |
| **Subnet**                | Firewall endpoint deployed in a dedicated subnet (must be in each AZ) |

---

## ✅ Use Cases

* ✅ Block access to malicious domains (e.g., `malware.com`)
* ✅ Allow only specific IPs or ports
* ✅ Deep inspection of HTTP/HTTPS traffic
* ✅ Centralized inspection for multi-VPC environment

---

# 🧪 Real-Time Lab: Block Outbound HTTP Traffic Using AWS Network Firewall

---

## 🎯 Scenario

You have a **public subnet** with an EC2 instance. You want to:

* Allow outbound access to HTTPS (443)
* **Block all HTTP (80)** traffic to external sites (e.g., `http://example.com`)

---

## 🔹 Lab Architecture

```
  Internet
     ▲
     │
 ┌───┴──────┐
 │ Network  │
 │ Firewall │
 └───▲──────┘
     │
 ┌───┴────────────┐
 │ VPC            │
 │ ┌────────────┐ │
 │ │ EC2 (Public)│ │
 │ └────────────┘ │
 └────────────────┘
```

---

## ✅ Step 1: VPC Setup

1. Create a **VPC** (CIDR: `10.0.0.0/16`)
2. Create two subnets:

   * **Public Subnet** (`10.0.1.0/24`) for EC2
   * **Firewall Subnet** (`10.0.2.0/24`) for firewall endpoint
3. Create and attach an **Internet Gateway**
4. Create and associate a **custom route table** for the **public subnet**

---

## ✅ Step 2: Launch EC2 Instance

1. Launch an **Amazon Linux 2** instance in the **public subnet**
2. Enable public IP
3. Inbound SG: allow **SSH (22)** from your IP

Install curl:

```bash
sudo yum install -y curl
```

---

## ✅ Step 3: Create AWS Network Firewall

1. Go to **VPC > Network Firewall > Firewalls**
2. Click **Create Firewall**

**Configuration**:

* Name: `BlockHTTPFirewall`
* VPC: select your VPC
* Firewall subnet: `10.0.2.0/24` (Firewall Subnet)
* Firewall policy: Create new

---

## ✅ Step 4: Create Firewall Policy

1. Name: `BlockHTTPPolicy`
2. Create a **stateless rule group**:

   * Name: `DropHTTPStateless`
   * Rule type: **Stateless**
   * Rule definition:

     ```json
     [
       {
         "RuleDefinition": {
           "Actions": [ "aws:drop" ],
           "MatchAttributes": {
             "Protocols": [6],
             "DestinationPorts": [ { "FromPort": 80, "ToPort": 80 } ]
           }
         },
         "Priority": 1
       }
     ]
     ```

> Protocol `6` = TCP, Port `80` = HTTP

3. Attach this stateless rule group to the policy under **stateless rules** in the **forwarding direction**

✅ Save the policy and associate with the firewall

---

## ✅ Step 5: Update Route Tables

1. Create a new **route table for the public subnet**:

   * Route to **Internet** via **Firewall endpoint**
2. Replace the existing IGW route in public subnet with:

   * **Target**: Firewall Endpoint
   * Destination: `0.0.0.0/0`

Now all outbound traffic from EC2 → routed through Network Firewall.

---

## ✅ Step 6: Test from EC2

SSH into the EC2 instance:

```bash
# Try HTTPS (should work)
curl https://example.com

# Try HTTP (should be blocked)
curl http://example.com
```

### ✅ Expected Output:

* HTTPS: Returns valid response
* HTTP: Hangs or shows connection timeout

---

## ✅ Step 7: Optional – Enable Logging

1. Go to **Firewall > Logging**
2. Add:

   * Log destination: CloudWatch Logs
   * Log types: **Alert and Flow**
3. View logs under **CloudWatch > Log Groups**

You’ll see logs for **dropped HTTP traffic**.

---

## 🧠 Teaching Summary Table

| Component       | Role                                        |
| --------------- | ------------------------------------------- |
| Firewall        | Enforces rules                              |
| Firewall Policy | Holds rule group references                 |
| Rule Groups     | Stateless rule blocks port 80               |
| Subnet Routing  | Ensures EC2 traffic passes through firewall |
| Logging         | Visual confirmation in CloudWatch           |

---

## 🖼️ PPT / Whiteboard Diagram

```
         [Internet]
             ▲
             │
   ┌─────────┴─────────┐
   │  AWS Network FW   │  ← blocks HTTP
   └─────────▲─────────┘
             │
      ┌──────┴──────┐
      │  EC2 (curl) │
      └─────────────┘
```

---

## 📘 Real-World Use Cases

| Use Case                  | Description                                     |
| ------------------------- | ----------------------------------------------- |
| Block Malware Domains     | Use Suricata DNS rules to block known bad sites |
| Allow Only Specific Ports | E.g., 443 only outbound                         |
| Deep Packet Inspection    | Detect threats inside packets                   |
| Shared Services Firewall  | Centralized inspection for many VPCs            |

---

## 🧹 Cleanup (Avoid Charges)

* Terminate EC2
* Delete firewall and firewall policy
* Remove log group from CloudWatch
* Delete route table and VPC


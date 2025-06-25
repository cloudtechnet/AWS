# ✅ Module 3: Advanced Networking Components

## 🔷 Topic: **Transit Gateway (TGW)**

---

## 🔹 What is AWS Transit Gateway?

### ✅ Definition:

AWS Transit Gateway is a **central hub** that connects **multiple VPCs and on-premises networks** through a **single gateway**. It simplifies and **scales your network topology**.

Think of it as a **cloud router** that removes the need for complex VPC peering.

---

## 🔹 Why Use Transit Gateway?

### ✅ Benefits:

* **Centralized Connectivity**: One-to-many architecture vs. VPC Peering’s one-to-one
* **Scalable**: Connect up to **5,000 VPCs** in a region
* **Simplified Management**: Single route table to manage all routes
* **Cross-Region & On-Premise Support**: Works with VPN and Direct Connect

---

## 🔁 Comparison: TGW vs VPC Peering

| Feature              | VPC Peering     | Transit Gateway         |
| -------------------- | --------------- | ----------------------- |
| Connectivity Type    | One-to-One      | One-to-Many (Hub-Spoke) |
| Scalability          | Manual, limited | 1000s of VPCs           |
| Route Management     | Complex         | Centralized             |
| Inter-Region Support | Partial         | Yes                     |
| Transit Routing      | Not supported   | Fully supported         |

---

## 🧪 Real-Time Scenario Example

### 📝 Use Case:

You’re working in a company where different departments have **separate VPCs**:

* VPC-A (HR systems) → `10.0.0.0/16`
* VPC-B (Finance systems) → `10.1.0.0/16`
* VPC-C (DevOps tools) → `10.2.0.0/16`

Instead of peering all 3 together (A ↔ B, A ↔ C, B ↔ C), you connect all three to a **Transit Gateway**, enabling **full mesh communication** via **one centralized hub**.

---

## 🚀 Step-by-Step Lab Guide: Setup Transit Gateway with 3 VPCs

---

### ✅ **Step 1: Create 3 Custom VPCs**

| VPC Name | CIDR Block  | Subnet      |
| -------- | ----------- | ----------- |
| VPC-A    | 10.0.0.0/16 | 10.0.1.0/24 |
| VPC-B    | 10.1.0.0/16 | 10.1.1.0/24 |
| VPC-C    | 10.2.0.0/16 | 10.2.1.0/24 |

Launch **1 EC2 instance** in each VPC for connectivity test (Amazon Linux 2).

---

### ✅ **Step 2: Create a Transit Gateway**

1. Go to **VPC Dashboard > Transit Gateways**
2. Click **Create Transit Gateway**
3. Name: `central-tgw`
4. Leave default settings (auto-accept ON, route table ON)
5. Click **Create Transit Gateway**

---

### ✅ **Step 3: Attach VPCs to Transit Gateway**

Repeat for each VPC:

1. Go to **Transit Gateway Attachments**
2. Click **Create TGW Attachment**
3. Choose:

   * **Transit Gateway**: `central-tgw`
   * **VPC**: Select VPC-A
   * **Subnets**: Choose one subnet (for each VPC)
4. Create Attachment

Repeat for VPC-B and VPC-C

> ⚠️ Wait for attachment **status: Available**

---

### ✅ **Step 4: Update VPC Route Tables**

For each VPC’s route table:

* VPC-A route table:

  * `10.1.0.0/16` → `tgw-xxxxxxxx`
  * `10.2.0.0/16` → `tgw-xxxxxxxx`

* VPC-B route table:

  * `10.0.0.0/16` → `tgw-xxxxxxxx`
  * `10.2.0.0/16` → `tgw-xxxxxxxx`

* VPC-C route table:

  * `10.0.0.0/16` → `tgw-xxxxxxxx`
  * `10.1.0.0/16` → `tgw-xxxxxxxx`

---

### ✅ **Step 5: Security Group Rules**

Allow **ICMP (ping)** and **SSH** between the VPC CIDR ranges:

* EC2 SG in VPC-A: allow `10.1.0.0/16`, `10.2.0.0/16`
* EC2 SG in VPC-B: allow `10.0.0.0/16`, `10.2.0.0/16`
* EC2 SG in VPC-C: allow `10.0.0.0/16`, `10.1.0.0/16`

---

### ✅ **Step 6: Test Communication**

From EC2 in **VPC-A**, SSH and ping to:

```bash
ping 10.1.1.10  # EC2 in VPC-B
ping 10.2.1.10  # EC2 in VPC-C
```

Repeat from all instances.

---

### ✅ Expected Output:

```bash
PING 10.1.1.10 (10.1.1.10) 56(84) bytes of data.
64 bytes from 10.1.1.10: icmp_seq=1 ttl=254 time=1.20 ms
64 bytes from 10.2.1.10: icmp_seq=2 ttl=254 time=1.18 ms
```

> ✅ All EC2s can reach each other privately through the **Transit Gateway**

---

## 🧠 Teaching Summary Table:

| Component        | Purpose                       |
| ---------------- | ----------------------------- |
| Transit Gateway  | Centralized router for VPCs   |
| Attachments      | Link VPCs to TGW              |
| TGW Route Tables | Control inter-VPC routing     |
| VPC Route Tables | Enable traffic to go via TGW  |
| Security Groups  | Permit ICMP/SSH between CIDRs |

---

## 🧰 Extra: Troubleshooting Checklist

| Problem                  | Possible Cause                          |
| ------------------------ | --------------------------------------- |
| Ping fails               | Route table missing TGW entry           |
| Timeout                  | Attachment not ready                    |
| ICMP blocked             | Security group not updated              |
| EC2s can’t resolve names | Use internal DNS or verify DHCP options |

---

## 🖼️ Visual Suggestions for PPT:

* **Hub-Spoke Architecture** Diagram

  ```
          [VPC-A]
             |
             |
        [Transit GW]
          /     \
      [VPC-B]  [VPC-C]
  ```



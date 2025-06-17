# ✅ **Module 2: Core Components of AWS VPC**

---

## 🔹 **1. Subnets (Public & Private)**

### ✅ What is a Subnet?

A **subnet** is a subdivision of your VPC’s IP address range (CIDR block). It helps in organizing and **isolating resources** in your VPC.

### ✅ Types of Subnets:

#### 📘 **Public Subnet**:

* Subnet **associated with a Route Table** that has a route to the **Internet Gateway (IGW)**
* Resources in this subnet (like EC2) can be accessed **from the internet**
* Used for web servers, bastion hosts

#### 📘 **Private Subnet**:

* No direct route to the internet
* Resources are **isolated** and only access internet via **NAT Gateway/Instance**
* Used for databases, application backends

### ✅ Example:

VPC CIDR: `10.0.0.0/16`

* Public Subnet: `10.0.1.0/24`
* Private Subnet: `10.0.2.0/24`

---

## 🔹 **2. Route Tables**

### ✅ What is a Route Table?

A **Route Table** contains a set of rules (routes) used to determine **where network traffic is directed**.

### ✅ Key Points:

* Each subnet must be **explicitly associated** with a Route Table
* One **main route table** is created by default
* Routes are evaluated **top to bottom**

### ✅ Example Routes:

| Destination | Target                             |
| ----------- | ---------------------------------- |
| 10.0.0.0/16 | local                              |
| 0.0.0.0/0   | igw-1234abcd (for internet access) |

### ✅ Common Scenarios:

* **Public Subnet**: Route to IGW (0.0.0.0/0 → IGW)
* **Private Subnet**: Route to NAT (0.0.0.0/0 → NAT Gateway)

---

## 🔹 **3. Internet Gateway (IGW)**

### ✅ What is an IGW?

An **Internet Gateway** is a horizontally scaled, redundant, and highly available **gateway attached to your VPC** that allows communication between instances in your VPC and the **Internet**.

### ✅ Key Points:

* Must be attached to VPC
* Route Table must have a route to IGW
* IGW supports **both inbound and outbound traffic**

### ✅ Use Case:

* Allows EC2 instances in public subnet to receive traffic from internet (e.g., web server)

---

## 🔹 **4. NAT Gateway & NAT Instance**

### ✅ What is NAT?

NAT (Network Address Translation) allows instances in a **private subnet to access the internet**, but **prevents inbound access** from the internet.

### ✅ 📌 Two Types:

#### 1. **NAT Gateway (Managed by AWS)**

* Highly available, managed service
* Auto scaling and no need to patch
* Requires an **Elastic IP**
* Attached in a **public subnet**

#### 2. **NAT Instance (Self-managed EC2)**

* You configure and manage updates
* Requires route configuration and security group rules
* Less scalable, not preferred for production

### ✅ Use Case:

* Allow EC2s in private subnet to:

  * Download software updates
  * Access external APIs

---

## 🔹 **5. Elastic IP (EIP)**

### ✅ What is Elastic IP?

An **Elastic IP** is a **static, public IPv4 address** that you can allocate to your AWS account and **associate with EC2, NAT Gateway, or other services**.

### ✅ Key Benefits:

* Remains the same across reboots and stops/starts
* You can move it between resources

### ✅ Use Cases:

* Assign a stable public IP to your EC2 (like Bastion Host)
* Associate with NAT Gateway for consistent internet egress

### ⚠️ Note:

* AWS charges for **unused EIPs**, so always release when not in use.

---

## 🔹 **6. DHCP Options Set**

### ✅ What is a DHCP Options Set?

A **DHCP Options Set** is a configuration set that defines **custom network settings** for EC2 instances launched in a VPC.

### ✅ You can specify:

* **Domain Name** (e.g., internal.company.com)
* **DNS Server IPs** (default: AmazonProvidedDNS)
* NTP servers
* NetBIOS name servers

### ✅ When is it used?

* If you want to:

  * Use **custom DNS servers** (like on-prem DNS)
  * Join instances to an **Active Directory domain**
  * Synchronize time using custom **NTP servers**


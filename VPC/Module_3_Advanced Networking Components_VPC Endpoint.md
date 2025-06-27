# ✅ Module 3: Advanced Networking Components

## 🔷 Topic: **VPC Endpoints (Gateway & Interface)**

---

## 🔹 What is a VPC Endpoint?

### ✅ Definition:

A **VPC Endpoint** enables you to **privately connect your VPC** to supported **AWS services** like S3, DynamoDB, or to **your own services** without using:

* Public IPs
* Internet Gateway (IGW)
* NAT Gateway

This ensures **traffic stays within the AWS network** (high security + low latency).

---

### ✅ Why Use VPC Endpoints?

* ✅ Increased **security**: No internet exposure
* ✅ Lower **data transfer cost** than NAT Gateway
* ✅ High **performance & low latency**
* ✅ Works with **IAM policies** for fine-grained access

---

## 🔹 Types of VPC Endpoints

| Endpoint Type          | Description                                                        | Example Services                     |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------ |
| **Gateway Endpoint**   | Target for a route table. Used for **S3** and **DynamoDB** only    | Amazon S3, DynamoDB                  |
| **Interface Endpoint** | ENI with a private IP in your subnet. Used for most other services | SSM, SNS, SQS, ECR, CloudWatch, etc. |

---

## ✅ Real-Time Use Case Example

### 🧾 Scenario:

You have an EC2 instance in a **private subnet** that needs to:

* Access **S3** → via **Gateway Endpoint**
* Access **SSM Parameter Store** → via **Interface Endpoint**

You will demonstrate both, step-by-step, and confirm that **internet is NOT required** to access these services.

---

# 🧪 Step-by-Step Lab: VPC Endpoints Setup

---

## 🔹 Lab Setup:

### ☁️ VPC Setup

* VPC CIDR: `10.0.0.0/16`
* Public Subnet: `10.0.1.0/24` (optional)
* Private Subnet: `10.0.2.0/24`
* NAT Gateway: ❌ Not used (to prove no internet access)
* EC2 in private subnet
* Attach an **IAM Role** with:

  * S3 read-only access
  * SSM full access

---

## 🔹 Step 1: Launch EC2 in Private Subnet

* Amazon Linux 2 instance
* No public IP
* Attach IAM role (mentioned above)
* Install AWS CLI and SSM Agent if not already present

---

## 🔹 Step 2: Create Gateway Endpoint for S3

1. Go to **VPC Console → Endpoints → Create Endpoint**
2. Name: `s3-gateway-endpoint`
3. Service category: **AWS services**
4. Service name: `com.amazonaws.<region>.s3`
5. Type: **Gateway**
6. VPC: Select your VPC
7. Route Tables: Select the **private subnet’s route table**
8. Policy: Full access (or use custom IAM policy)
9. Create Endpoint

---

### ✅ Validate Gateway Endpoint

1. SSH into EC2 using Session Manager (via SSM)
2. Run:

   ```bash
   aws s3 ls
   ```

   or

   ```bash
   aws s3 ls s3://your-bucket-name
   ```

### ✅ Expected Output:

```bash
2025-06-10  my-data-bucket
2025-06-12  app-backups-bucket
```

> 🎯 SUCCESS: You accessed **S3 from a private subnet without Internet/NAT**.

---

## 🔹 Step 3: Create Interface Endpoint for SSM

1. Go to **VPC Console → Endpoints → Create Endpoint**
2. Name: `ssm-interface-endpoint`
3. Service Name: `com.amazonaws.<region>.ssm`
4. Type: **Interface**
5. VPC: Select your VPC
6. Subnets: Choose your **private subnet**
7. Security Group: Allow **TCP port 443**
8. Enable DNS name resolution
9. Create Endpoint

> Repeat for:

* `com.amazonaws.<region>.ssmmessages`
* `com.amazonaws.<region>.ec2messages`
  (these are also required for full SSM functionality)

---

### ✅ Validate Interface Endpoint (SSM)

1. Go to **Systems Manager → Session Manager**
2. Start a session with EC2 in the private subnet

> 🎯 SUCCESS: Instance connects via **SSM Endpoint**, not internet.

You can also test Parameter Store:

```bash
aws ssm put-parameter --name "/my/param" --value "DevValue" --type String
aws ssm get-parameter --name "/my/param"
```

### ✅ Expected Output:

```json
{
    "Parameter": {
        "Name": "/my/param",
        "Type": "String",
        "Value": "DevValue"
    }
}
```

---

## 🔐 Security Note:

* You can **restrict access to buckets or parameters** using **VPC endpoint policies**
* Endpoints support **IAM + VPC security** for extra control

---

## 🧠 Teaching Summary Table

| Type      | Use For           | Route Table? | Uses ENI? | Supports DNS |
| --------- | ----------------- | ------------ | --------- | ------------ |
| Gateway   | S3, DynamoDB      | ✅ Yes        | ❌ No      | ❌ No         |
| Interface | Most AWS services | ❌ No         | ✅ Yes     | ✅ Yes        |

---

## 🧰 Troubleshooting Tips

| Problem              | Solution                                     |
| -------------------- | -------------------------------------------- |
| Access denied        | Check IAM Role & endpoint policy             |
| S3 access fails      | Check route table has `S3 Endpoint` route    |
| SSM session fails    | Ensure all 3 interface endpoints are created |
| DNS resolution fails | Ensure endpoint DNS name is enabled          |




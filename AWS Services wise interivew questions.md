# AWS Services wise interivew questions

### **1. Amazon EC2 (Elastic Compute Cloud)**

1. What are the different types of EC2 instances, and how do you choose the right one for your workload?
2. How do you resize an EC2 instance, and what are the implications of doing so?
3. Explain the difference between on-demand, reserved, and spot instances.
4. What is the purpose of EC2 key pairs? How are they created and used?
5. How would you secure an EC2 instance against unauthorized access?
6. What is the difference between instance store and EBS-backed instances?
7. Explain Elastic IPs and how they are used in EC2.
8. How do you troubleshoot EC2 instance connectivity issues?
9. Can you attach multiple network interfaces to a single EC2 instance? If so, what are the use cases?
10. What are placement groups in EC2? Describe their types and use cases.

---

### **2. ELB (Elastic Load Balancer)**

1. What are the different types of ELBs, and when should you use each one?
2. How does an ELB handle incoming traffic? 
3. Explain the difference between connection draining and deregistration delay in ELB.
4. How do you configure SSL termination on an ELB?
5. What is a target group in the context of an Application Load Balancer?
6. How does health check work in ELB?
7. Can you use an ELB across multiple availability zones or regions? Why or why not?
8. What are sticky sessions, and how are they implemented in ELB?
9. What metrics can be monitored for ELB using CloudWatch?
10. How do you handle path-based and host-based routing in an Application Load Balancer?

---

### **3. EBS (Elastic Block Store)**

1. What are the different types of EBS volumes, and when would you use each one?
2. How do you increase the size or change the type of an EBS volume attached to an instance?
3. What happens to the data on an EBS volume when the associated EC2 instance is terminated?
4. How do you ensure the durability of data stored in an EBS volume?
5. Can you attach a single EBS volume to multiple EC2 instances? If yes, explain how.
6. What is the difference between an EBS snapshot and an AMI?
7. Explain the encryption options available for EBS volumes.
8. How do you migrate an EBS volume to a different region?
9. What is the performance impact of taking an EBS snapshot?
10. What is the purpose of the IOPS metric in EBS, and how do you calculate the required IOPS?

---

### **4. Auto Scaling**

1. What is the difference between dynamic scaling and predictive scaling in Auto Scaling?
2. How do you define scaling policies in Auto Scaling?
3. What is a launch configuration, and how does it differ from a launch template?
4. Can you use Auto Scaling for instances in multiple regions?
5. How do you configure health checks for Auto Scaling groups?
6. What is the lifecycle of an Auto Scaling instance?
7. How do you handle cooldown periods in Auto Scaling?
8. How does Auto Scaling integrate with ELB?
9. What metrics can trigger scaling in Auto Scaling groups?
10. Explain how to implement an Auto Scaling group to ensure high availability.

---

### **5. IAM (Identity and Access Management)**

1. What are IAM users, groups, and roles? How are they used?
2. How does IAM policy work? Explain the structure of a policy document.
3. What is the difference between an inline policy and a managed policy?
4. How do you use IAM roles with EC2 instances?
5. What are IAM access keys, and how should they be managed securely?
6. Explain the concept of IAM federation.
7. How do you troubleshoot an "Access Denied" error in AWS?
8. What are service-linked roles, and why are they used?
9. How do you implement the principle of least privilege in IAM?
10. What are the security best practices for IAM?

---

### **6. Route 53**

1. What are the different routing policies in Route 53?
2. How does Route 53 handle health checks?
3. Explain the difference between Alias Records and CNAME records.
4. How do you configure a DNS failover using Route 53?
5. Can you use Route 53 with non-AWS resources? If yes, how?
6. What is a hosted zone in Route 53?
7. How does Route 53 integrate with ELB?
8. What is the purpose of latency-based routing in Route 53?
9. How do you secure a Route 53 hosted zone?
10. What is the TTL value in DNS, and how does it affect Route 53 records?

---

### **7. VPC (Virtual Private Cloud)**

1. What are the components of a VPC?
2. How do you configure a public and private subnet in a VPC?
3. What is the difference between a NAT instance and a NAT gateway?
4. Explain how security groups and network ACLs work in a VPC.
5. What is the difference between an internet gateway and an egress-only internet gateway?
6. How do you establish a VPN connection to a VPC?
7. What is VPC peering, and how is it implemented?
8. How do you troubleshoot connectivity issues in a VPC?
9. What is AWS Transit Gateway, and how does it work with VPCs?
10. Explain how to design a highly available architecture in a VPC.

---

### **8. RDS (Relational Database Service)**

1. What are the supported database engines in RDS?
2. Explain the difference between RDS and Aurora.
3. What is a read replica in RDS, and how is it used?
4. How do you implement Multi-AZ deployments in RDS?
5. What are parameter groups and option groups in RDS?
6. How do you back up and restore an RDS database?
7. What is RDS encryption, and how is it enabled?
8. Can you scale RDS instances vertically and horizontally? Explain how.
9. What is the difference between manual snapshots and automated backups in RDS?
10. How do you monitor RDS performance metrics?

---

### **9. CloudWatch & CloudTrail**

#### *CloudWatch:*
1. What are the key components of CloudWatch?
2. How do you create a CloudWatch alarm to monitor CPU utilization?
3. What is the difference between CloudWatch metrics, logs, and events?
4. How do you enable detailed monitoring for an EC2 instance?
5. How does CloudWatch integrate with Auto Scaling?

#### *CloudTrail:*
6. What is the primary purpose of CloudTrail?
7. How do you configure a CloudTrail to log API activity across multiple regions?
8. What is the difference between a CloudTrail log file and an event in CloudWatch?
9. How do you secure CloudTrail logs stored in S3?
10. What are the use cases for CloudTrail Insights?

---


# Automated-EC2-Lifecycle-and-Snapshot-Management-using-AWS-Lambda-and-Boto3
---

## 🧾 **Overview:**

This project automates essential EC2 instance lifecycle operations—**creation**, **start**, **stop**, **reboot**, and **terminate**—along with **automated snapshot creation** for EC2 volumes. The automation is implemented using **AWS Lambda functions written in Python with Boto3**, scheduled using **Amazon EventBridge (cron expressions)**. Proper **IAM roles and permissions** are attached to ensure secure and controlled access.

---

## 🔧 **Technologies Used:**

* **AWS Lambda**
* **Amazon EC2**
* **Amazon EBS**
* **Amazon EventBridge (CloudWatch Events)**
* **IAM Roles and Policies**
* **Python (Boto3 SDK)**

---

## ⚙️ **Features Implemented:**

### 1. **EC2 Instance Management:**

* **Create EC2 Instance**

  * Using pre-defined AMI, instance type, key pair, and security groups.
* **Start EC2 Instance**

  * Starts a stopped EC2 instance by instance ID.
* **Stop EC2 Instance**

  * Gracefully stops a running EC2 instance.
* **Reboot EC2 Instance**

  * Reboots a running EC2 instance.
* **Terminate EC2 Instance**

  * Permanently deletes an EC2 instance and detaches volumes.

### 2. **Automated Snapshot Management:**

* **Create Snapshots of EBS Volumes**

  * Periodically creates snapshots for attached volumes of running instances.
  * Tags snapshots with instance ID and timestamp.

### 3. **Automation with EventBridge (Cron Jobs):**

| Task              | Frequency     | Cron Expression       |
| ----------------- | ------------- | --------------------- |
| Start Instance    | Daily at 7 AM | `cron(30 1 * * ? *)`  |
| Stop Instance     | Daily at 8 PM | `cron(30 14 * * ? *)` |
| Snapshot Creation | Every 6 hours | `cron(0 */6 * * ? *)` |

---

## 🔐 **IAM Roles & Permissions:**

A custom IAM Role with the following permissions was created and attached to each Lambda function:

### Required Permissions:

* `ec2:StartInstances`
* `ec2:StopInstances`
* `ec2:RebootInstances`
* `ec2:TerminateInstances`
* `ec2:RunInstances`
* `ec2:DescribeInstances`
* `ec2:CreateSnapshot`
* `ec2:DescribeVolumes`
* `ec2:CreateTags`
* `logs:*` (for CloudWatch logging)

---

## 🧑‍💻 **Lambda Functions Overview:**

Each Lambda function is written in **Python 3.x** using **Boto3**, and deployed with the appropriate IAM role and triggers.

### Example Snippet: Start Instance

```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2', region_name='us-east-1')
    instance_id = 'i-0abc123def456gh78'
    ec2.start_instances(InstanceIds=[instance_id])
    return {
        'statusCode': 200,
        'body': f'Started EC2 instance {instance_id}'
    }
```

---

## 📊 **Outcome:**

* Fully automated EC2 lifecycle management.
* No manual intervention required to maintain uptime and backups.
* Enhanced cost efficiency by scheduling stop/terminate.
* Daily backup snapshots stored for disaster recovery.

---

## 📌 **Future Enhancements:**

* Add snapshot retention logic (e.g., delete snapshots older than X days).
* Email/SNS notifications on job execution.
* Dashboard using CloudWatch metrics for visual tracking.

  ------

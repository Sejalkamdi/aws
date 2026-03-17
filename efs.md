# 🚀 Practical: Implementing Amazon EFS with Multiple EC2 Instances

## 🎯 Objective

To create an **Amazon Elastic File System (EFS)** and mount it on **multiple EC2 instances** so they can share the same storage.

---

# ⚙️ Step-by-Step Implementation

## 🧩 Step 1: Create the EFS File System

1. Log in to the **AWS Management Console**.
2. Navigate to **Amazon Elastic File System**.
3. Click **Create File System**.
4. Select the **VPC** where your EC2 instances will run.
5. Keep the default configuration or customize if needed.
6. add same security group as your instance.
7. Click **Create File System**.

📌 AWS automatically creates **Mount Targets** inside the VPC so instances can connect to the EFS.

---

## 🔐 Step 2: Configure Security Groups

To allow proper communication between EC2 and EFS, add the following inbound rules:

| Type | Protocol | Port | Purpose                      |
| ---- | -------- | ---- | ---------------------------- |
| SSH  | TCP      | 22   | Allows SSH access to EC2     |
| NFS  | TCP      | 2049 | Allows EC2 to connect to EFS |

📌 **SSH** is required for remote login.
📌 **NFS (Network File System)** is required for mounting the EFS storage.

---

## 💻 Step 3: Launch Multiple EC2 Instances

1. Open the **Amazon EC2 Dashboard.
2. Launch **two or more Linux instances** (Ubuntu / Amazon Linux).
3. Make sure all instances:

   * Are in the **same VPC**
   * Use the **same security group**
4. These instances will access the **same EFS storage**.

---

## 🔑 Step 4: Connect to EC2 Using SSH

Use your key pair to connect to the instance.

```bash
ssh -i key.pem ubuntu@<public-ip>
```

Repeat this step for each EC2 instance.

---

## 📦 Step 5: Install the NFS Client

The NFS client allows the instance to communicate with EFS.

### Ubuntu

```bash
sudo apt update
sudo apt install nfs-common -y
```

### Amazon Linux

```bash
sudo yum install nfs-utils -y
```

---

## 📁 Step 6: Create a Mount Directory

Create a directory where the EFS will be attached.

```bash
sudo mkdir /efs
```

---

## 🔗 Step 7: Mount the EFS File System

Mount the EFS using the DNS name provided in the AWS console.

```bash
sudo mount -t nfs4 <efs-dns-name>:/ /efs
```

Example:

```bash
sudo mount -t nfs4 fs-12345678.efs.ap-south-1.amazonaws.com:/ /efs
```

📌 Run this command on **all EC2 instances** to connect them to the same file system.

---

## ✅ Step 8: Verify the Mount

Check whether the file system is successfully mounted.

```bash
df -h
```

You should see **EFS mounted on ****`/efs`**.

---

## 🧪 Step 9: Test the Shared Storage

### On Instance 1

```bash
cd /efs
sudo touch sharedfile.txt
ls
```

### On Instance 2

```bash
cd /efs
ls
```

If **sharedfile.txt** appears on the second instance, it confirms that **EFS is successfully sharing storage between EC2 instances**.

---

## 🔄 Step 10: Make the Mount Permanent (Optional)

To automatically mount EFS after reboot:

```bash
sudo nano /etc/fstab
```

Add the following line:

```
<efs-dns-name>:/ /efs nfs4 defaults,_netdev 0 0
```

Save and exit.

Now the EFS will **automatically mount whenever the instance starts**.

---

# 🏁 Result

Amazon EFS was successfully created and mounted on **multiple EC2 instances**, enabling **shared and scalable cloud storage**.

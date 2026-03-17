# 💾 AWS Practical – EBS Disk Partition

This practical demonstrates how to **create an Amazon EBS volume, attach it to an EC2 instance, create disk partitions, and mount it to the system.**

---

# 🚀 Practical Steps

## 🖥️ Step 1 – Launch an EC2 Instance

1. Open **AWS Console**.
2. Go to **EC2 Dashboard**.
3. Click **Launch Instance**.
4. Select **Ubuntu / Amazon Linux**.
5. Choose instance type **t2.micro**.
6. Create or select a **key pair**.
7. In **Security Group**, allow:

   * **SSH (Port 22)**
8. Click **Launch Instance**.

---

## 💽 Step 2 – Create an EBS Volume

1. Open **EC2 Dashboard**.
2. Click **Volumes** under **Elastic Block Store**.
3. Click **Create Volume**.
4. Configure:

* Volume Type → **gp3**
* Size → **10 GiB**
* Availability Zone → **Same as EC2**

5. Click **Create Volume**.

---

## 🔗 Step 3 – Attach the Volume

1. Select the created **volume**.
2. Click **Actions → Attach Volume**.
3. Select your **EC2 instance**.
4. Device name:

```
/dev/xvdf
```

5. Click **Attach**.

---

## 🔑 Step 4 – Connect to EC2

Connect using SSH.

```bash
ssh -i key.pem ubuntu@<public-ip>
```

---

## 🔍 Step 5 – Check the New Disk

Run:

```bash
lsblk
```

Example output:

```
xvda
└─xvda1

xvdf
```

`xvdf` is the new **EBS disk**.

---

## 🧱 Step 6 – Create Disk Partition

Run:

```bash
sudo fdisk /dev/xvdf
```

Inside the tool type:

```
n
p
1
Enter
Enter
+20G
w
```

This creates **partition xvdf1**.

---

## 📦 Step 7 – Format the Partition

```bash
sudo mkfs.ext4 /dev/xvdf1
```

This prepares the disk for storing files.

---

## 📁 Step 8 – Create Mount Folder

```bash
sudo mkdir /data
```

---

## 🔗 Step 9 – Mount the Partition

```bash
sudo mount /dev/xvdf1 /data
```

---

## ✅ Step 10 – Verify the Mount

```bash
df -h
```

You will see something like:

```
/dev/xvdf1   10G   24M   9.9G   /data
```

---

## 🧪 Step 11 – Test Storage

```bash
cd /data
touch testfile.txt
ls
```

If `testfile.txt` appears, the **EBS storage is working**.

---

# 🏁 Result

Successfully created an **EBS volume**, partitioned the disk, formatted it, and mounted it on the EC2 instance.

---

# ⚡ Important Commands

```bash
lsblk
sudo fdisk /dev/xvdf
sudo mkfs.ext4 /dev/xvdf1
sudo mkdir /data
sudo mount /dev/xvdf1 /data
df -h
```
## 🔄 Step 12 – Enable Automatic Mounting (Permanent Mount)

Currently, the EBS disk is mounted **temporarily**.  
If the EC2 instance **restarts**, the mounted disk will be **removed automatically**.

To make sure the disk **mounts automatically every time the system starts**, we need to configure the **fstab file**.

---

### 📝 1. Open the fstab Configuration File

```bash
sudo vim /etc/fstab
➕ 2. Add the Disk Entry

At the bottom of the file, add the following line:

/dev/xvdf1 /data ext4 defaults,nofail 0 2

This entry tells Linux to:

Mount /dev/xvdf1

At the directory /data

Using the ext4 filesystem

Automatically during system startup

💾 3. Save and Exit


🔍 4. Verify the Configuration

Run the following command to test the configuration:

sudo mount -a

If no error appears, the configuration is correct.

✅ The EBS partition will now automatically mount to /data every time the EC2 instance restarts.

tep-by-Step Procedure for AWS RDS Practical
Log in to AWS Management Console.
Navigate to RDS service → Click Create database.
Select Standard Create.
Choose engine type (MySQL/PostgreSQL).
Select version and template (Free tier/Dev/Test).
Enter DB instance identifier.
Set master username and password.
Choose instance class (e.g., db.t3.micro).
Configure storage (default settings or as required).
Select VPC.
Choose or create DB subnet group.
Select Availability Zone (or leave default).
Set Public access = Yes/No as required.
Create or select security group.
Add inbound rule:
Type: MySQL/Aurora
Port: 3306
Source: EC2 security group or IP
Disable/Enable Multi-AZ as required.
Click Create database.
Wait until status shows Available.
Launch EC2 instance (if not already created).
Connect to EC2 via SSH.
Install MySQL client:
sudo apt update
sudo apt install mysql-client -y
Copy RDS endpoint from RDS dashboard.
Connect to RDS from EC2:
mysql -h <endpoint> -u <username> -p
Enter password when prompted.
Create database:
CREATE DATABASE testdb;
Use database:
USE testdb;
Create table:
CREATE TABLE users (id INT, name VARCHAR(50));
Insert data:
INSERT INTO users VALUES (1, 'Alice');
Retrieve data:
SELECT * FROM users;
Go to RDS console → Select DB instance.
Click Actions → Create Read Replica.
Configure replica settings → Click Create.
Modify DB → Enable Multi-AZ (if not enabled).
Apply changes → Wait for modification to complete.
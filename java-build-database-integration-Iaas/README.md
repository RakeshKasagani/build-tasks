🧰 Prerequisites

✅ AWS Account

✅ Java Web Application (.war file) — e.g., myapp.war

✅ MySQL-compatible app (JDBC connection in web.xml or context.xml)

✅ SSH key pair for EC2

✅ Security groups configured properly

🏗️ STEP 1: Launch EC2 Instance for Tomcat

Login to AWS Console → EC2 → Launch Instance
<img width="631" height="320" alt="Image" src="https://github.com/user-attachments/assets/dee156fe-d55a-45d5-bc0e-6792e0d5604b" />

Choose Ubuntu or Amazon Linux AMI

Choose an instance type (e.g., t3.micro for testing)

Create or use an existing key pair

Add security group rules:

✅ Port 22 (SSH) – your IP

✅ Port 8080 (Tomcat)

Launch the instance and connect via SSH:

ssh -i yourkey.pem ubuntu@<EC2-Public-IP>

☕ STEP 2: Install Java and Tomcat on EC2
# Update packages
```bash
sudo apt update -y
```


# Install Java
sudo apt install openjdk-17-jdk -y   # or Java 11 depending on your app

# Check version
java -version

# Install Tomcat 9
sudo apt install tomcat9 tomcat9-admin -y

# Start Tomcat
sudo systemctl start tomcat9
sudo systemctl enable tomcat9

# Check status
sudo systemctl status tomcat9


✅ Access Tomcat in browser:
http://<EC2-Public-IP>:8080
(You should see Tomcat’s default page.)

🛢️ STEP 3: Create MySQL Database on AWS RDS

Go to RDS → Create database

Engine: MySQL

Choose Free Tier (if eligible)

DB instance identifier: mydb

Master username & password (remember these)

Set VPC and Public Access = Yes (or keep private and use VPC peering if needed)

Add Security group allowing inbound:

✅ Port 3306 from EC2 Security Group

Launch DB instance

Wait until status = Available.

✅ Note down:

Endpoint (e.g., mydb.abcdefgh.us-east-1.rds.amazonaws.com)

Port (3306)

Username / Password

🧱 STEP 4: Connect EC2 → RDS MySQL

From EC2 SSH:

sudo apt install mysql-client -y
mysql -h <RDS-ENDPOINT> -u <username> -p


If successful, you’re connected.

✅ Create your app database and tables:

CREATE DATABASE myappdb;
USE myappdb;
-- Example table
CREATE TABLE users (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(50));

⚙️ STEP 5: Configure Web Application Database Connection

There are 2 common ways:

Option A: JDBC in your application code

In your Java servlet/config file:

String url = "jdbc:mysql://mydb.abcdefgh.us-east-1.rds.amazonaws.com:3306/myappdb";
String username = "admin";
String password = "StrongPassword";
Connection con = DriverManager.getConnection(url, username, password);

Option B: JNDI (context.xml / web.xml)

In META-INF/context.xml:

<Resource name="jdbc/MyDB" 
          auth="Container" 
          type="javax.sql.DataSource"
          maxTotal="100" 
          maxIdle="30" 
          maxWaitMillis="10000"
          username="admin" 
          password="StrongPassword"
          driverClassName="com.mysql.cj.jdbc.Driver"
          url="jdbc:mysql://mydb.abcdefgh.us-east-1.rds.amazonaws.com:3306/myappdb"/>

🪄 STEP 6: Install MySQL Connector/J

Tomcat needs the MySQL driver:

cd /var/lib/tomcat9/lib
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.4.0.tar.gz
tar -xvf mysql-connector-j-8.4.0.tar.gz
cd mysql-connector-j-8.4.0
sudo cp mysql-connector-j-8.4.0.jar /usr/share/tomcat9/lib/


Restart Tomcat:

sudo systemctl restart tomcat9

📦 STEP 7: Deploy the WAR File

Copy your .war file to Tomcat webapps:

sudo cp myapp.war /var/lib/tomcat9/webapps/


Tomcat will auto-deploy it.

Check the logs if needed:

sudo tail -f /var/log/tomcat9/catalina.out


✅ Access the application:
http://<EC2-Public-IP>:8080/myapp

🔐 STEP 8: (Optional) Secure with Security Groups & Environment Variables

Restrict RDS to EC2 Security Group only (not 0.0.0.0/0)

Use AWS Secrets Manager or .env file for DB credentials

Use a reverse proxy (like Nginx) to map port 80 → 8080 if needed.

sudo apt install nginx -y
sudo nano /etc/nginx/sites-available/default

location / {
    proxy_pass http://localhost:8080/myapp;
}


Then:

sudo systemctl restart nginx


Access:
http://<EC2-Public-IP>

🧪 STEP 9: Test the Application

Form submission should write to RDS DB

Data should persist after Tomcat restart

You can monitor DB using:

mysql -h <RDS-ENDPOINT> -u <username> -p
USE myappdb;
SELECT * FROM users;

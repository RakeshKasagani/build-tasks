📜 Overview of Architecture
┌─────────────────────┐        ┌─────────────────────────────┐
│   User (Browser)    │        │     AWS RDS (MySQL DB)      │
│  http://publicIP    │───────▶│   users table (login data)  │
└─────────────────────┘        └─────────────────────────────┘
           ▲
           │
           ▼
┌─────────────────────┐
│  EC2 (Tomcat 9)     │
│  Deploy WAR file    │
│  Connect to RDS     │
└─────────────────────┘

✅ STEP 1: Set Up AWS Infrastructure
1.1 Launch EC2 Instance

Go to AWS EC2 → Launch Instance
<img width="631" height="320" alt="1" src="https://github.com/user-attachments/assets/350976fb-fe93-4d28-8fbc-08070e5d8f53" />


AMI: Ubuntu 22.04 LTS (or Amazon Linux 2)
<img width="611" height="355" alt="2" src="https://github.com/user-attachments/assets/2077ec08-a68f-4cc4-b529-abf4da59151b" />


Instance type: t3.micro (Free tier)
<img width="631" height="246" alt="3" src="https://github.com/user-attachments/assets/46214390-740a-4a77-8e9b-31a7dba21021" />


Key pair: Create/select key
<img width="400" height="350" alt="4" src="https://github.com/user-attachments/assets/c06d7340-92e7-4313-a1ef-938c2bd9e36d" />


Security Group:

Allow SSH (22) from your IP

Allow mysql (3306)

Allow Tomcat (8080)
<img width="628" height="382" alt="1" src="https://github.com/user-attachments/assets/2bd1bfea-54f4-4c6c-8da9-327b0582f2f3" />
<img width="631" height="343" alt="2" src="https://github.com/user-attachments/assets/ade15c6d-eb94-4195-8d8f-4f7034cbe333" />


Launch and connect via SSH:

ssh -i your-key.pem ubuntu@<EC2-Public-IP>

🐳 STEP 2: Install Java & Tomcat on EC2
sudo apt update -y
sudo apt install openjdk-17-jdk -y    # or Java 11
java -version

sudo apt install tomcat9 tomcat9-admin -y
sudo systemctl enable tomcat9
sudo systemctl start tomcat9


Check:

http://<EC2-Public-IP>:8080


You should see the Tomcat home page ✅

🧱 STEP 3: Create AWS RDS MySQL Database

Go to RDS → Create Database

Engine: MySQL

Templates: Free tier

DB Instance identifier: login-db

Master user: admin
Password: StrongPassword123!

Public access: Yes (for demo; in production use private)

Security Group: allow inbound port 3306 from EC2 security group.

Launch DB

✅ After creation, note:

RDS Endpoint (e.g., login-db.abcdefgh.ap-south-1.rds.amazonaws.com)

Port: 3306

🛢 STEP 4: Create Database & Table

From your EC2 instance:

sudo apt install mysql-client -y
mysql -h <RDS-ENDPOINT> -u admin -p


Inside MySQL:

CREATE DATABASE login_db;
USE login_db;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) NOT NULL,
  password VARCHAR(50) NOT NULL
);

INSERT INTO users (username, password) VALUES ('admin', 'admin123');


✅ Test:

SELECT * FROM users;

📦 STEP 5: Prepare Java Login Web Application
Directory structure example:
LoginApp/
├── src/
│   └── LoginServlet.java
├── WebContent/
│   ├── index.jsp
│   ├── login.jsp
│   └── WEB-INF/
│       └── web.xml

Sample LoginServlet.java:
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.sql.*;

public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        String jdbcURL = "jdbc:mysql://login-db.abcdefgh.ap-south-1.rds.amazonaws.com:3306/login_db";
        String dbUser = "admin";
        String dbPassword = "StrongPassword123!";

        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection conn = DriverManager.getConnection(jdbcURL, dbUser, dbPassword);
            String sql = "SELECT * FROM users WHERE username=? AND password=?";
            PreparedStatement stmt = conn.prepareStatement(sql);
            stmt.setString(1, username);
            stmt.setString(2, password);
            ResultSet rs = stmt.executeQuery();

            if(rs.next()) {
                out.println("<h2>Login Successful! Welcome " + username + "</h2>");
            } else {
                out.println("<h2>Invalid credentials</h2>");
            }

            conn.close();
        } catch(Exception e) {
            e.printStackTrace(out);
        }
    }
}

web.xml:
<web-app>
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>LoginServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
</web-app>

📥 STEP 6: Add MySQL Connector/J Driver

Download MySQL Connector:

cd /usr/share/tomcat9/lib
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.4.0.tar.gz
tar -xvf mysql-connector-j-8.4.0.tar.gz
cd mysql-connector-j-8.4.0
sudo cp mysql-connector-j-8.4.0.jar /usr/share/tomcat9/lib/


Restart Tomcat:

sudo systemctl restart tomcat9

🏗 STEP 7: Build & Deploy the WAR File

If you used Eclipse / Maven:

Export project as LoginApp.war

On EC2:

scp -i your-key.pem LoginApp.war ubuntu@<EC2-Public-IP>:/tmp
sudo cp /tmp/LoginApp.war /var/lib/tomcat9/webapps/


Tomcat will auto-deploy:

/var/lib/tomcat9/webapps/LoginApp/

🌐 STEP 8: Access the Application

Open in browser:

http://<EC2-Public-IP>:8080/LoginApp


Enter:

Username: admin
Password: admin123


✅ If successful → it will display:

Login Successful! Welcome admin


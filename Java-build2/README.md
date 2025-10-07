🚀 Java Web Application: Build & Deploy Guide (Java 17 + Maven + JBoss/WildFly)
🧾 Overview

This project is a Java Web Application built using Java 17 and Maven, and deployed on JBoss (WildFly) application server.
It demonstrates a simple Java web app structure — a calculator web application using JSP and Servlet components.

📁 Project Structure
NETFLIX/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── in/raham/servlets/
│   │   │       └── CalculatorServlet.java
│   │   ├── resources/
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       ├── index.jsp
│   │       └── styles.css
│   └── test/
│       └── java/
│           └── in/raham/tests/
│               └── CalculatorTest.java
└── README.md
Folder explanation:

pom.xml → Maven project configuration file

CalculatorServlet.java → Java logic for your calculator

web.xml → Web deployment descriptor (inside WEB-INF)

index.jsp → Home page of your web app

CalculatorTest.java → JUnit test cases

target/ → Folder automatically created after Maven build (contains .war file)


⚙️ Prerequisites
✅ Step 1: Install Java 17
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```
<img width="631" height="320" alt="Image" src="https://github.com/user-attachments/assets/d1cc766b-99b0-4566-88ad-9b795543d792" />
<img width="611" height="355" alt="2" src="https://github.com/user-attachments/assets/791c2198-80f4-4913-8c0b-9dbc784ad167" />
<img width="631" height="246" alt="3" src="https://github.com/user-attachments/assets/0ba95995-e875-470e-ad8e-55d72dabf408" />
<img width="400" height="350" alt="4" src="https://github.com/user-attachments/assets/b20cb5b1-47aa-4a4b-99e9-ce46e8962cb5" />
<img width="923" height="347" alt="5" src="https://github.com/user-attachments/assets/bff215dc-e471-493b-b2b3-5cdfe5fddd65" />
<img width="564" height="342" alt="6" src="https://github.com/user-attachments/assets/b5613acc-773d-41a0-86e6-3d3a9c13bd8c" />
<img width="673" height="137" alt="7" src="https://github.com/user-attachments/assets/28cff599-4a7c-441a-891f-f212d5173ff8" />
<img width="827" height="215" alt="8" src="https://github.com/user-attachments/assets/eab4c865-1409-45b2-bb98-5c10e2271016" />








Verify:

java -version


Expected output:

openjdk version "17.0.x" ...

✅ Step 2: Install Maven
sudo apt install maven -y


Verify:

mvn -v

✅ Step 3: Install JBoss / WildFly

Download and extract WildFly (JBoss):

wget https://github.com/wildfly/wildfly/releases/download/32.0.0.Final/wildfly-32.0.0.Final.tar.gz
tar -xvzf wildfly-32.0.0.Final.tar.gz
mv wildfly-32.0.0.Final /opt/wildfly


Start the JBoss server:

cd /opt/wildfly/bin
./standalone.sh -b 0.0.0.0



Access the JBoss console:

http://<server-ip>:8080

🧩 Step 4: Update pom.xml

Ensure your pom.xml contains Java 17 configurations.
You can use this preconfigured version:

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>


Then run:

mvn clean package


This will generate a .war file inside:

target/NETFLIX.war

🚀 Step 5: Deploy WAR File to JBoss
Option 1: Deploy via JBoss Admin Console

Open your browser:
👉 http://<server-ip>:8080


Click Enable to activate the deployment.

Option 2: Deploy via Command Line (CLI)

Copy WAR file to the JBoss deployments directory:

scp -i git.pem /home/ubuntu/JavaWebCalculator/target/*.war \
ubuntu@<JBOSS_SERVER_IP>:/opt/wildfly/standalone/deployments/


JBoss automatically detects new .war files in the deployments directory and deploys them automatically.

🧪 Step 6: Test the Application

Open your browser and visit:

http://<JBOSS_SERVER_IP>:8080/NETFLIX.war


Application running in browser



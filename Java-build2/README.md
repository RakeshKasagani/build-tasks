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
<img width="845" height="146" alt="9" src="https://github.com/user-attachments/assets/514debe3-1aa2-485a-af60-f98c68f36a30" />



Verify:

mvn -v

✅ Step 3: Install JBoss / WildFly

Download and extract WildFly (JBoss):
```bash
wget https://github.com/wildfly/wildfly/releases/download/32.0.0.Final/wildfly-32.0.0.Final.tar.gz
tar -xvzf wildfly-32.0.0.Final.tar.gz
```
<img width="856" height="140" alt="7" src="https://github.com/user-attachments/assets/b827eeda-50fb-4cae-bfaf-eb1e26b68e1e" />

mv wildfly-32.0.0.Final /opt/wildfly

```bash
Start the JBoss server:

```
./standalone.sh -b 0.0.0.0
<img width="850" height="362" alt="10" src="https://github.com/user-attachments/assets/91943c3b-14e2-4937-aa9c-28033a611e1b" />




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
```bash
mvn clean package
```
<img width="851" height="198" alt="12" src="https://github.com/user-attachments/assets/6076af56-02ec-4c97-bb00-7efd472d345b" />
<img width="585" height="152" alt="13" src="https://github.com/user-attachments/assets/94d7346c-d8b3-498f-8742-3f55ed801399" />



This will generate a .war file inside:
```bash
target/NETFLIX.war
```
<img width="841" height="222" alt="14" src="https://github.com/user-attachments/assets/3132872f-750f-44ac-a8b7-77d647f441c8" />


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
```bash

http://<JBOSS_SERVER_IP>:8080/NETFLIX.war
```
<img width="956" height="462" alt="11" src="https://github.com/user-attachments/assets/ab5c3807-378c-41e3-80d4-f88975935bf6" />

Application running in browser



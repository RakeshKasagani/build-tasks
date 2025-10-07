#  Java Web Application: Build & Deploy Guide (Java 17 + Maven + Tomcat)

## Overview
This project is a **Java Web Application** designed to run on **Java 17**, built using **Maven**, and deployed on **Apache Tomcat**. The project demonstrates a basic Java web app structure, with a simple calculator implementation.

## Project Structure
```bash
JavaWebCalculator/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/mypackage/Calculator.java
│   │   └── webapp/
│   │       ├── WEB-INF/web.xml
│   │       └── index.jsp
│   └── test/java/mypackage/CalculatorTest.java

```

- **pom.xml**: Maven project configuration.
- **Calculator.java**: Java class for your calculator logic.
- **index.jsp**: The homepage of the web application.
- **web.xml**: Web application configuration.
- **CalculatorTest.java**: Unit tests for the calculator.

## Prerequisites

### Step 1: Install Java 17

To install Java 17, run the following commands (for Ubuntu/Debian):

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
<img width="644" height="83" alt="Image" src="https://github.com/user-attachments/assets/4a452fcc-d8cc-408c-ba19-13cc19934aa6" />
<img width="628" height="190" alt="Image" src="https://github.com/user-attachments/assets/2b321f2b-d90d-4cee-9fd2-22d6110696e5" />
```
## java -version
### Output should be something like:
### openjdk version "17.0.x"...

## Install Maven
```bash
sudo apt install maven -y
<img width="647" height="111" alt="Image" src="https://github.com/user-attachments/assets/0e93a3cf-4e12-405f-8754-5efa94e98e48" />
```
## Verify Maven installation
```bash
mvn -v
```

## Install Apache Tomcat
```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.110/bin/apache-tomcat-9.0.110.tar.gz
tar -xvzf apache-tomcat-9.0.110.tar.gz
<img width="932" height="255" alt="9" src="https://github.com/user-attachments/assets/c291c934-9d40-4b5d-9f21-b7ddadf6d02b" />

```
## Add Tomcat User for Manager Access
```bash
sudo vi tomcat/conf/tomcat-users.xml
```
## Add the following inside <tomcat-users>
```bash
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="admin" password="admin123" roles="manager-gui,manager-script"/>
```
## Edit the context.xml file to allow remote access
```bash
sudo vi tomcat/webapps/manager/META-INF/context.xml
<img width="647" height="143" alt="11" src="https://github.com/user-attachments/assets/f2da6a45-3dba-44bf-9ec1-e32ee0fd5e0e" />

```
## Comment out the IP restriction
```bash
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1" />
-->
```
## Update pom.xml for Java 17 is available in the pom.xml file in this repository

## Clean and Package the Application
### Navigate to your project directory
```bash
cd JavaWebCalculator
```
### Run the following Maven command to clean and package your application
```bash
mvn clean package
<img width="584" height="176" alt="12" src="https://github.com/user-attachments/assets/03414e26-8813-4a35-bd9c-1dce99682bf9" />
<img width="792" height="455" alt="13" src="https://github.com/user-attachments/assets/e4a84d03-7b49-4558-962f-3a0784c04fbe" />
<img width="833" height="250" alt="14" src="https://github.com/user-attachments/assets/c15db37d-9232-45c2-9364-e3616b4db5bf" />



```

## Deploy the WAR to Apache Tomcat
### Option 1: Deploy via Manager Web Interface

### Go to: http://localhost:8080/manager/html

### Login using your tomcat/tomcat credentials.

### In the Deploy section, upload the .war file: target/webapp-0.2.war.

### Click Deploy.

## Deploy the WAR to Apache Tomcat
Transfer the WAR File to the Deploy Server

To copy the .war file from the build server to the deploy server, use WinSCP and PuTTY to transfer the .pem file and then use scp to copy the .war file.

Use WinSCP and PuTTY to upload the .pem file to your deploy server.

On the build server, use the following scp command to copy the .war file to the deploy server
```bash
scp -i git.pem /home/ubuntu/JavaWebCalculator/target/*.war ubuntu@3.231.144.26:/home/ubuntu/apache-tomcat-9.0.110/webapps/
```
## Test the Application
Open your browser and visit:

http://localhost:8080/webapp-0.2
<img width="833" height="250" alt="14" src="https://github.com/user-attachments/assets/23c03f82-3f71-4c61-9011-0e2f71cce5fd" />
<img width="596" height="431" alt="13" src="https://github.com/user-attachments/assets/2709edff-b8e2-40a1-a512-1777689de6df" />



You should see the index.jsp page of your web application.


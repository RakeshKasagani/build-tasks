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
```
## java -version
### Output should be something like:
### openjdk version "17.0.x"...

## Install Maven
```bash
sudo apt install maven -y
```
## Verify Maven installation
```bash
mvn -v
```

## Install Apache Tomcat
```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.110/bin/apache-tomcat-9.0.110.tar.gz
tar -xvzf apache-tomcat-9.0.110.tar.gz
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
```

## Deploy the WAR to Apache Tomcat
### Option 1: Deploy via Manager Web Interface

### Go to: http://localhost:8080/manager/html

### Login using your admin/admin123 credentials.

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

You should see the index.jsp page of your web application.
## Screenshots

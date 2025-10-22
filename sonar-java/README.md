🧠 SonarQube Integration with Java Documentation (Ubuntu 24.04 Server)
⚙️ Server Setup Overview
🖥️ Environment

OS: Ubuntu 24.04

Instance: AWS EC2

Security Groups:

SSH → Port 22 (for remote access)

SonarQube → Port 9000 (for web access)

✅ Installed Components
Software	Version	Purpose
Java	17+	Required by SonarQube
SonarQube	Latest LTS (10.x)	Code Quality Analysis
PostgreSQL (optional)	14+	SonarQube database
Maven	3.9.x	For Java builds
Node & npm	18.x / 9.x	For JavaScript builds
🧩 1. Installing and Configuring SonarQube
Step 1: Install Java
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version

Step 2: Create SonarQube User
sudo adduser sonar
sudo usermod -aG sudo sonar

Step 3: Download and Setup SonarQube
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.899.zip
sudo apt install unzip -y
sudo unzip sonarqube-10.6.0.899.zip
sudo mv sonarqube-10.6.0.899 sonarqube
sudo chown -R sonar:sonar /opt/sonarqube

Step 4: Run SonarQube

Switch to sonar user and start service:

sudo su - sonar
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start


Check status:

./sonar.sh status

Step 5: Access SonarQube

Go to browser:

http://<your-server-ip>:9000


Default login:

Username: admin
Password: admin


You will be asked to change the password.

Step 6: Generate Authentication Token

Login → My Account → Security → Generate Token

Example Token:

squ_495df877b4d1017dabaf457233c6fa3626fc56ea

🧱 2. Integration with Java Code
🧠 Overview

SonarQube analyzes Java code for:

Code smells

Bugs

Security vulnerabilities

Test coverage (if configured with JaCoCo)

We’ll integrate with a Maven project using the Sonar Maven Plugin.

🧾 Example Values
Parameter	Value
Project Name	Java-App
SonarQube Server URL	http://54.234.208.154:9000
Token	squ_495df877b4d1017dabaf457233c6fa3626fc56ea
Source Directory	src/main/java
📂 Folder Structure (Typical Maven Project)
Java-App/
├── pom.xml
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/example/App.java
│   └── test/
│       └── java/
│           └── com/example/AppTest.java
└── sonar-project.properties (optional)

⚙️ Step 1: Add Sonar Plugin to pom.xml

Add inside <build> section:

<build>
  <plugins>
    <plugin>
      <groupId>org.sonarsource.scanner.maven</groupId>
      <artifactId>sonar-maven-plugin</artifactId>
      <version>3.9.1.2184</version>
    </plugin>
  </plugins>
</build>

⚙️ Step 2: Create sonar-project.properties (Optional)

If you want external configuration:

sonar.projectKey=Java-App
sonar.projectName=Java-App
sonar.projectVersion=1.0
sonar.sources=src/main/java
sonar.language=java
sonar.sourceEncoding=UTF-8
sonar.host.url=http://54.234.208.154:9000
sonar.login=squ_495df877b4d1017dabaf457233c6fa3626fc56ea

⚙️ Step 3: Run SonarQube Scan

From project root:

mvn clean verify sonar:sonar \
  -Dsonar.projectKey=Java-App \
  -Dsonar.host.url=http://54.234.208.154:9000 \
  -Dsonar.login=squ_495df877b4d1017dabaf457233c6fa3626fc56ea

⚙️ Step 4: Verify in SonarQube Dashboard

Visit → http://54.234.208.154:9000/projects

Click on Java-App

Check:

✅ Bugs

⚠️ Code smells

🔒 Security vulnerabilities

🧪 Test coverage (if configured)

🧩 Step 5: Automate in CI/CD (Optional)

In Jenkins or GitHub Actions:

mvn clean verify sonar:sonar \
  -Dsonar.host.url=http://54.234.208.154:9000 \
  -Dsonar.login=$SONAR_TOKEN


Store $SONAR_TOKEN securely.

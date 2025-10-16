📁 Project Setup

Build Server (EC2): t3.micro → Java 17 + Maven

Deploy Server (EC2): t3.micro → Java 17 + Tomcat

Nexus Server (EC2): t3.medium → Java 17 + Nexus Repository (Port 8081)

🧱 1. BUILD SERVER (Artifact Generation)
Step 1: SSH into Build EC2
ssh -i my-key.pem ubuntu@<BUILD_SERVER_PUBLIC_IP>

Step 2: Update Packages and Install Java 17
sudo apt update -y
sudo apt upgrade -y
sudo apt install openjdk-17-jdk -y
java -version

Step 3: Install Maven
sudo apt install maven -y
mvn -version

Step 4: Create or Clone Java Application
git clone https://github.com/your-repo/your-java-app.git
cd your-java-app


OR create a Maven project:

mvn archetype:generate \
 -DgroupId=com.example \
 -DartifactId=MyApp \
 -DarchetypeArtifactId=maven-archetype-webapp \
 -DinteractiveMode=false
cd MyApp

Step 5: Add version in pom.xml

In pom.xml:

<groupId>com.example</groupId>
<artifactId>MyApp</artifactId>
<version>1.0.0</version>  <!-- You can change version for each build -->
<packaging>war</packaging>


Example for next build:

1.0.0 (first version)

1.0.1 (bug fix or new change)

1.1.0 (feature update)

Step 6: Build the Artifact
mvn clean package


This will generate:

target/MyApp-1.0.0.war


📌 You can increment the version in pom.xml for each new build to maintain artifact versions.

📦 2. NEXUS SERVER (Artifact Storage)
Step 1: SSH into Nexus EC2
ssh -i my-key.pem ubuntu@<NEXUS_SERVER_PUBLIC_IP>

Step 2: Update and Install Java
sudo apt update -y
sudo apt install openjdk-17-jdk -y

Step 3: Install Nexus
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -zxvf latest-unix.tar.gz
sudo mv nexus-3* /opt/nexus
sudo useradd nexus
sudo chown -R nexus:nexus /opt/nexus
sudo vi /opt/nexus/bin/nexus.rc
# Add:
run_as_user="nexus"

sudo su - nexus
/opt/nexus/bin/nexus start

Step 4: Access Nexus Dashboard

Open browser:

http://<NEXUS_SERVER_PUBLIC_IP>:8081

Step 5: Login

Default credentials:

user: admin

password: /opt/sonatype-work/nexus3/admin.password

Change password and create repositories.

Step 6: Create a Maven Hosted Repository

Go to Settings → Repositories

Click “Create repository” → maven2 (hosted)

Enter:

Name: maven-releases

Version Policy: Release

Write Policy: Allow Redeploy

Save.

Step 7: Configure Maven to use Nexus

Edit ~/.m2/settings.xml on Build Server:

<settings>
  <servers>
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>your_password</password>
    </server>
  </servers>
</settings>


In your project pom.xml add:

<distributionManagement>
    <repository>
        <id>nexus</id>
        <url>http://<NEXUS_SERVER_PUBLIC_IP>:8081/repository/maven-releases/</url>
    </repository>
</distributionManagement>

Step 8: Deploy Artifact to Nexus
mvn clean deploy


✅ This will upload:

http://<NEXUS_SERVER_PUBLIC_IP>:8081/repository/maven-releases/com/example/MyApp/1.0.0/MyApp-1.0.0.war


Next time, change version in pom.xml to 1.0.1 and run deploy again.

🚀 3. DEPLOY SERVER (Tomcat Deployment)
Step 1: SSH into Deploy EC2
ssh -i my-key.pem ubuntu@<DEPLOY_SERVER_PUBLIC_IP>

Step 2: Install Java 17
sudo apt update -y
sudo apt install openjdk-17-jdk -y

Step 3: Install Tomcat 9
cd /opt
sudo wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.93/bin/apache-tomcat-9.0.93.tar.gz
sudo tar -xvf apache-tomcat-9.0.93.tar.gz
sudo mv apache-tomcat-9.0.93 tomcat9
cd tomcat9/bin
chmod +x *.sh

Step 4: Start Tomcat
./startup.sh


Check:

http://<DEPLOY_SERVER_PUBLIC_IP>:8080

Step 5: Download Artifact from Nexus
cd /opt/tomcat9/webapps
wget http://<NEXUS_SERVER_PUBLIC_IP>:8081/repository/maven-releases/com/example/MyApp/1.0.0/MyApp-1.0.0.war --user=admin --password=your_password


OR if no authentication is required, direct wget.

Step 6: Tomcat Auto Deployment

Tomcat automatically deploys any WAR placed inside webapps folder.

ls /opt/tomcat9/webapps
# MyApp-1.0.0.war should be here


Check:

http://<DEPLOY_SERVER_PUBLIC_IP>:8080/MyApp-1.0.0/


For next version:

Remove old war and exploded folder

Download new version from Nexus:

rm -rf MyApp-1.0.0* 
wget http://<NEXUS_SERVER_PUBLIC_IP>:8081/repository/maven-releases/com/example/MyAp

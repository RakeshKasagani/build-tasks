💻 Sonar Integration with JavaScript Code
🧠 Overview

SonarQube can analyze JavaScript, TypeScript, Angular, React, or Node.js projects.
You’ll use the SonarScanner CLI tool.

🧾 Example Values
Parameter	Value
Project Name	Angular-js
SonarQube Server URL	http://54.234.208.154:9000
Token	squ_495df877b4d1017dabaf457233c6fa3626fc56ea
Source Directory	src
📂 Folder Structure
Angular-js/
├── src/
│   ├── app/
│   │   └── components/
│   └── index.js
├── package.json
└── sonar-project.properties

⚙️ Step 1: Install Node & npm (if not installed)
sudo apt update
sudo apt install nodejs npm -y
node -v
npm -v

⚙️ Step 2: Install SonarScanner CLI
sudo apt install unzip -y
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
unzip sonar-scanner-cli-5.0.1.3006-linux.zip
sudo mv sonar-scanner-5.0.1.3006-linux /opt/sonar-scanner
echo 'export PATH=$PATH:/opt/sonar-scanner/bin' >> ~/.bashrc
source ~/.bashrc


Check version:

sonar-scanner -v

⚙️ Step 3: Create sonar-project.properties

In project root:

sonar.projectKey=Angular-js
sonar.projectName=Angular-js
sonar.projectVersion=1.0

sonar.sources=src
sonar.language=js
sonar.sourceEncoding=UTF-8

sonar.host.url=http://54.234.208.154:9000
sonar.login=squ_495df877b4d1017dabaf457233c6fa3626fc56ea

⚙️ Step 4: Run the Scanner
cd /path/to/Angular-js
sonar-scanner


You can also pass parameters inline:

sonar-scanner \
  -Dsonar.projectKey=Angular-js \
  -Dsonar.sources=src \
  -Dsonar.host.url=http://54.234.208.154:9000 \
  -Dsonar.login=squ_495df877b4d1017dabaf457233c6fa3626fc56ea

⚙️ Step 5: View in SonarQube Dashboard

Go to:

http://54.234.208.154:9000/projects


You’ll see Angular-js listed.
Click the project to explore:

🪲 Bugs

💡 Code smells

🔒 Security issues

🔁 Duplications

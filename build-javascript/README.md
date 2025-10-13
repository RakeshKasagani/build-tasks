**🚀 Deploying a JavaScript (Angular) Application using Node, NPM, and Nginx**

This guide walks through the process of setting up a development environment, building an Angular application, and deploying it to a production server using Nginx.

--------------------------------------------------------------------------------------------------------------------------------------------

**🧰 Prerequisites**

Before you begin, ensure you have the following:

Two Ubuntu EC2 instances (or VMs):

Development Server – for building the Angular app

Production Server – for hosting with Nginx

SSH key pair for secure access (.pem file)

Basic knowledge of Linux commands

------------------------------------------------------------------------------------------------------------------------------------------

⚙️ 1. Setting up the Development Server
Step 1: Set Hostname and Update Packages
```
sudo hostnamectl set-hostname dev
sudo init 6
sudo apt update -y
```

Step 2: Install Node.js and NPM

To ensure compatibility, install the latest stable Node.js (v22.x):
```
sudo apt remove -y nodejs
sudo apt autoremove -y
sudo apt purge -y nodejs npm
sudo rm -rf /usr/local/lib/node_modules
sudo rm -f /usr/local/bin/node /usr/local/bin/npm
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify installation:
```
node -v
npm -v
```
<img width="254" height="76" alt="Image" src="https://github.com/user-attachments/assets/7212e810-d4f2-4d94-b85d-2c98b9e051b5" />


Step 3: Install Angular CLI
```
sudo npm install -g @angular/cli
ng version
```



🏗️ 2. Building the Angular Application

Navigate to your Angular project directory:
```
git clone 
cd ~/AngularCalculator
```
<img width="512" height="119" alt="Image" src="https://github.com/user-attachments/assets/e0f2cf45-c21c-418b-8cb1-5be3cebb4e5d" />

Install dependencies:
```
sudo npm install
```
<img width="675" height="116" alt="Image" src="https://github.com/user-attachments/assets/94759b34-f310-4c09-9d0b-c3aa576af503" />

Build the project for production:
```
ng build --configuration production
```

📝 If you face OpenSSL issues during build, use:
```
NODE_OPTIONS=--openssl-legacy-provider ng build --prod
```
<img width="705" height="193" alt="Image" src="https://github.com/user-attachments/assets/c9c050e7-8698-424b-bbda-92dac79d7220" />

After building, your output files will be located at:
```
AngularCalculator/dist/angularCalc/
```


📤 3. Transferring Build Files to the Production Server
Step 1: Set permissions for the .pem key
```
chmod 400 .pemkey
```

Step 2: Securely copy build files to production server
```
scp -i db.pem -r AngularCalculator/dist/angularCalc/* ubuntu@<PROD_SERVER_IP>:/var/www/html/
```

<img width="761" height="120" alt="15" src="https://github.com/user-attachments/assets/1b1623f1-b844-42bc-871e-a45fe8844e0f" />

<img width="743" height="62" alt="16" src="https://github.com/user-attachments/assets/6e25914f-92ba-451b-afc4-c9b7b7d31d7a" />



🌐 4. Setting up the Production Server (Nginx)
Step 1: Set Hostname and Update
```
sudo hostnamectl set-hostname prod
sudo init 6
sudo apt update -y
```

Step 2: Install Nginx
```
sudo apt install nginx -y
nginx --version
sudo systemctl start nginx
```
<img width="763" height="243" alt="18" src="https://github.com/user-attachments/assets/ca536fec-1116-4946-a2c4-b4fd8d8c6926" />


Step 3: Set Permissions for Web Root
```
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod -R 777 /var/www/html
```

Step 4: Restart and Check Nginx
```
sudo systemctl restart nginx
sudo systemctl status nginx
```
<img width="765" height="248" alt="17" src="https://github.com/user-attachments/assets/be43789f-12a3-48db-8424-b27c881af8f1" />



**✅ 5. Verify Deployment**
Open your browser and visit:
```
http://<PRODUCTION_SERVER_PUBLIC_IP>/
```

**🧾 Summary of Key Commands**
| Task              | Command                                                 |
| ----------------- | ------------------------------------------------------- |
| Update packages   | `sudo apt update -y`                                    |
| Install Node.js   | `sudo apt install -y nodejs`                            |
| Build Angular app | `ng build --configuration production`                   |
| Copy build files  | `scp -i <key>.pem -r dist/* ubuntu@<ip>:/var/www/html/` |
| Install Nginx     | `sudo apt install nginx -y`                             |
| Restart Nginx     | `sudo systemctl restart nginx`                          |



**📦 Folder Structure**
<pre> ```
AngularCalculator/
├── src/
├── dist/
│   └── angularCalc/
├── package.json
└── angular.json
 ``` </pre>


**🏁 Final Workflow Overview**

Development Server (Build Phase)
→ Install Node.js + Angular CLI
→ Build Angular Project
→ Transfer Build Files

Production Server (Deploy Phase)
→ Install and Configure Nginx
→ Host Angular App in /var/www/html
→ Access via Public IP



**🎯 Outcome**

You’ve successfully:

Set up a Node.js + Angular build environment

Compiled your app for production

Hosted your Angular app using Nginx







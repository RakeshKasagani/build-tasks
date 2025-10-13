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

<img width="1422" height="240" alt="Image" src="https://github.com/user-attachments/assets/fdf1e813-7262-4de1-85c6-2cb9105dcbb1" />
<img width="1314" height="116" alt="Image" src="https://github.com/user-attachments/assets/fc394f0e-bb58-4896-9bed-cc8412b96fe9" />


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
<img width="1466" height="425" alt="Screenshot 2025-10-13 180649" src="https://github.com/user-attachments/assets/b37d920a-9c06-43d7-ab54-4285d0c4252b" />

<img width="462" height="57" alt="Image" src="https://github.com/user-attachments/assets/60c14476-90da-4515-9dba-f231968408ac" />


Step 3: Set Permissions for Web Root
```
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod -R 777 /var/www/html
```

Step 4: Configure Nginx

Open the default configuration file:
```
sudo vi /etc/nginx/sites-available/default
```

Modify it as follows:
```
server {
    listen 80;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

<img width="591" height="347" alt="Image" src="https://github.com/user-attachments/assets/ae97cd87-0eaa-46fe-b240-3a54a11fe6b0" />


Step 5: Restart and Check Nginx
```
sudo systemctl restart nginx
sudo systemctl status nginx
```
<img width="1443" height="425" alt="Image" src="https://github.com/user-attachments/assets/ef6d9e52-8ae4-49b8-ae82-c856bec90020" />


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




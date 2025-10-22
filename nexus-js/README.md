🧩 Detailed Documentation: Nexus Repository Integration for JavaScript

⚙️ 1. Prerequisites

✅ Ensure the following are ready:

Requirement	Description
Nexus Repository	Installed & accessible (port 8081)
Host URL	Example: http://54.234.208.200:8081
Repository Type	npm (hosted)
Access Credentials	username and password for Nexus login
Node.js + npm	Installed on your system

Check:

node -v
npm -v

🏗️ 2. Create npm Repository in Nexus

Log in to Nexus via browser:

http://54.234.208.200:8081


Go to:
Administration → Repository → Repositories → Create repository

Choose npm (hosted) type.

Configure:

Name: npm-releases

Deployment policy: Allow redeploy

Blob store: default (or custom)

Save the repository.

Copy the repository URL (important!):

http://54.234.208.200:8081/repository/npm-releases/

📦 3. Configure Your JavaScript Project

Navigate to your project directory (example: AngularCalculator):

cd ~/AngularCalculator


Your folder should look like:

AngularCalculator/
├── src/
├── package.json
└── .npmrc

⚙️ 4. Configure .npmrc File

Create or edit the .npmrc file in your project root:

touch .npmrc
nano .npmrc


Add the following lines:

registry=http://54.234.208.200:8081/repository/npm-releases/

# Authentication (base64 encoded username:password)
_auth=$(echo -n 'admin:admin123' | base64)
always-auth=true
email=admin@example.com


Alternatively, you can add manually (without shell):

registry=http://54.234.208.200:8081/repository/npm-releases/
_auth=YWRtaW46YWRtaW4xMjM=    # base64 for admin:admin123
always-auth=true
email=admin@example.com

✅ Example .npmrc (Final)
registry=http://54.234.208.200:8081/repository/npm-releases/
_auth=YWRtaW46YWRtaW4xMjM=
always-auth=true
email=admin@example.com

🧾 5. Update package.json

Edit your package.json and make sure it has:

{
  "name": "angular-calculator",
  "version": "1.0.0",
  "description": "Angular Calculator App",
  "main": "index.js",
  "scripts": {
    "build": "ng build",
    "test": "ng test"
  },
  "publishConfig": {
    "registry": "http://54.234.208.200:8081/repository/npm-releases/"
  }
}


The publishConfig key ensures the package always publishes to your Nexus registry.

🚀 6. Authenticate npm (Optional Global Login)

If you prefer not to use _auth in .npmrc, you can login interactively:

npm login --registry=http://54.234.208.200:8081/repository/npm-releases/


Then enter:

Username: admin
Password: admin123
Email: admin@example.com


This will automatically create or update your .npmrc.

🏗️ 7. Build Your JavaScript Package

If you have a build process (e.g., Angular, Node, React):

Angular:

npm run build


Node.js:

npm pack


This creates a .tgz package file inside your project folder.

📤 8. Publish Package to Nexus

Once .npmrc is configured, run:

npm publish


✅ Expected Output:

npm notice 
npm notice 📦  angular-calculator@1.0.0
npm notice === Tarball Contents === 
npm notice 1.1kB package.json
npm notice 2.2kB index.js
npm notice === Tarball Details === 
npm notice Name: angular-calculator
npm notice Version: 1.0.0
npm notice Tarball: angular-calculator-1.0.0.tgz
npm notice === Published ===
+ angular-calculator@1.0.0

🧭 9. Verify in Nexus

Go to Nexus UI → Browse → npm-releases

You should see your package (example: angular-calculator)

Click to view its metadata (version, size, tarball, etc.)

🔁 10. Use Your Private Nexus npm Registry

To install packages from your Nexus repo:

npm install angular-calculator --registry=http://54.234.208.200:8081/repository/npm-releases/


If you want this to apply globally (for all npm installs):

npm set registry http://54.234.208.200:8081/repository/npm-releases/


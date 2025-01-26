# MERN-Stack-App-Deployment-on-AWS-Cloud
Deploying MERN stack Application on AWS cloud 

### Agenda

1) Install Node.js
2) Deploy Node.js App
3) Connect Domain
4) Implement SSL

Steps to Deploy MERN Stack App

**1. Create an EC2 Instance**
  - Instance Configuration:
  - OS: Ubuntu
  - Instance Type: 2 CPUs, 1 GB memory
  - Allow SSH port
  - EBS Volume: 8 GB
  - SSH inside that 

**2.Run the following commands to install Node.js using NVM:**

### Install NVM
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

### Export NVM Directory
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### Check NVM Installation
```
nvm
```

### Install Node.js (LTS Version)
```
nvm install --lts
```

### Verify Node.js Version
```
node -v
```

**3. Deploy Backend Node.js App**
**Optional**: Create a Basic Node.js App
### Create and Initialize App
```
mkdir myapp
```

```
cd myapp
```
```
npm init -y
```
### Install Express Framework
```
npm i express
```

### Create app.js File
```
sudo vim app.js
```

### Add the following code in app.js:
```
const express = require("express");
const app = express();
const port = 3000;

app.get("/", (req, res) => {
    res.send("Hey, the server is running!");
});

app.listen(port, () => {
    console.log("Server is running on port " + port);
});
```

### Run the Application
```
node app.js
```

# Note: or we can use git clone url if we have existing backend application 


### Note:Security Group Configuration
**Allow HTTP and HTTPS traffic from anywhere.**
**Add a custom TCP rule to allow traffic on port 3000.** 

**Access the App**

**Visit: http://<EC2-Public-IP>:3000**

### Create .env file inside backend application location 
```
sudo vim .env
```

### Add the MongoDB connection string to the .env file.
**Note: ** basically add your remote server url **
```
MONGO_URI=mongodb+srv://account:<username>Pass<password>mongo.nrehc.mongodb.net/myDatabase?retryWrites=true&w=majority
PORT=3000
```



### Whitelist EC2 Public IP in MongoDB
- Login to MongoDB server.
- Go to Network Access and whitelist your EC2 Public IP.


****Note: Till here our Backend app is running at foreground

**4. Run Application at background Install and use PM2 to run the app in the background:**
# Install PM2 Globally
```
npm i -g pm2
```

# Start the App Using PM2
```
pm2 start app.js
```


**5. Install and Configure NGINX**
```
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl status nginx
```
### Visit: http://<EC2-Public-IP> to see the default NGINX page.
Configure NGINX
```
cd /etc/nginx/sites-available
sudo vim default
```

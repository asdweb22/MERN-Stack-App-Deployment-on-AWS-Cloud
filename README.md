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

# Note: or we can use git clone url if we have existing backend application go inside backend app make sure server.js file or app.js file over there and do 
**it will install required dependencies which are required to run this backend app**
```
npm i
``` 


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
or
```
pm2 start server.js
```


**5. Install and Configure NGINX & frontend deployment **
```
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl status nginx
```
### Visit: http://<EC2-Public-IP> to see the default NGINX page.


**Deploy Frontend React App**
- go to frontend location
- install required dependencies
  ```
  npm i
  ```
  **Build the React app :**
  ```
  npm run build
  ```

**move this build to location **
```
sudo cp -r dist /var/www/html
```




**Update NGINX Configuration:**
```
cd /etc/nginx/sites-available
sudo vim default
```


**Update the default file:**
```
server {
    listen 80 default_server;
    server_name _;

    location / {
        root /var/www/html/dist;
        index index.html;
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Restart NGINX:**
```
sudo systemctl reload nginx
```

**Visit: http://<EC2-Public-IP> to see the app.**



**Important Notes**

- Frontend Deployment: Ensure the dist folder is moved to /var/www/html.
- Make sure update inside frontend code wherever you calling backend api make sure check public ip /static ip of your running app
- Backend API Integration: Update the frontend API calls to use the backend public IP.
- Use below command when you updated inside backend app want to see latest changes 
  ```
  pm2 reload all
  ```
- SSL Renewal: Use certbot renew to renew SSL certificates automatically.

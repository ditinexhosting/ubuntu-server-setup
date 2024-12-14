# ubuntu-server-setup

ssh -i ~/.ssh/ailabs.pem azureuser@20.127.200.44

sudo apt update

sudo apt-get install nodejs -y

sudo apt install nginx

sudo apt install certbot python3-certbot-nginx

sudo npm install pm2 -g

sudo apt-get install -y mongodb-org

### Create new DB
mongosh
use fnol
db.createUser({ user: "fnoldbuser", pwd: "95rPFn996WwP7jYl", roles: ["readWrite"]})

nano /etc/mongod.conf
security
  authorization: enabled

mongodb://fnoldbuser:95rPFn996WwP7jYl@127.0.0.1:27017/fnol?authSource=fnol

```
server {
    server_name cirigirilabs.ai;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log debug;

    location / {
        proxy_pass http://localhost:3000;
	      proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	      proxy_set_header X-Forwarded-Proto $scheme;	
	      proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Sec-WebSocket-Protocol "chat, superchat";
	      proxy_cache                     off;
        # Headers for client browser NOCACHE + CORS origin filter 
        add_header 'Cache-Control' 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';
        expires off;
        add_header    'Access-Control-Allow-Methods' 'GET, POST, OPTIONS' always;
        add_header    'Access-Control-Allow-Headers' 'Origin, X-Requested-With, Content-Type, Accept' always;

    }

    location /api/ {
        proxy_pass http://localhost:3001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;        
        proxy_set_header X-Forwarded-Proto $scheme;     
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Sec-WebSocket-Protocol "chat, superchat";
	      proxy_cache                     off;
        # Headers for client browser NOCACHE + CORS origin filter 
        add_header 'Cache-Control' 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';
        expires off;
        add_header    'Access-Control-Allow-Methods' 'GET, POST, OPTIONS' always;
        add_header    'Access-Control-Allow-Headers' 'Origin, X-Requested-With, Content-Type, Accept' always;
    }
}
```

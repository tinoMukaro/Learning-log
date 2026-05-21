# NGINX Notes

## What is NGINX?

NGINX is a web server that can also work as:

- Reverse proxy
- Load balancer
- API gateway
- Static file server
- SSL/TLS termination server
- Caching server

NGINX is popular because it is fast, lightweight, and handles many connections efficiently.

---

# Main Uses of NGINX

1. Static Web Server

NGINX can serve files like:

- HTML
- CSS
- JavaScript
- Images
- Videos

Example:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

2. Reverse Proxy

A reverse proxy receives requests from users and forwards them to another server/app.

Example:

User → NGINX → Spring Boot / Node.js / React app

```ngix
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://localhost:8080;
    }
}
```

This means NGINX receives traffic on port 80 and forwards it to the app running on port 8080.

3. Load Balancer

NGINX can distribute traffic between multiple backend servers.

```ngix
upstream backend_servers {
    server localhost:8081;
    server localhost:8082;
    server localhost:8083;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_servers;
    }
}
```

This helps when one app has multiple running instances.

NGINX Configuration Files

Main config file:

/etc/nginx/nginx.conf

Common site config folders:

/etc/nginx/sites-available/
/etc/nginx/sites-enabled/

Usually:

sites-available stores config files
sites-enabled contains active configs
Active configs are often linked from sites-available

Example:

```ngix
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
Basic nginx.conf Structure
user www-data;

worker_processes auto;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;

    server {
        listen 80;
        server_name example.com;

        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```

Important NGINX Blocks

1. main block

Global settings.

user www-data;
worker_processes auto; 2. events block

Controls connection handling.

events {
worker_connections 1024;
} 3. http block

Contains web server settings.

http {
include /etc/nginx/mime.types;
} 4. server block

Represents one website or app.

server {
listen 80;
server_name example.com;
} 5. location block

Controls how specific routes are handled.

location /api {
proxy_pass http://localhost:8080;
}
Common NGINX Settings
listen

Defines the port NGINX listens on.

listen 80;
listen 443 ssl;
server_name

Defines the domain name.

server_name example.com www.example.com;
root

Defines the folder where static files are stored.

root /var/www/html;
index

Defines the default file.

index index.html index.htm;
location

Matches request paths.

location / {
try_files $uri $uri/ =404;
}
proxy_pass

Forwards requests to another server.

proxy_pass http://localhost:8080;
Reverse Proxy Config Example
server {
listen 80;
server_name myapp.com;

    location / {
        proxy_pass http://localhost:3000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

}
What the headers mean
proxy_set_header Host $host;

Keeps the original domain name.

proxy_set_header X-Real-IP $remote_addr;

Passes the real client IP address.

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

Keeps a chain of forwarded IP addresses.

proxy_set_header X-Forwarded-Proto $scheme;

Tells the backend whether the request used HTTP or HTTPS.

React App NGINX Config

For a React app after running:

npm run build

You can serve the dist or build folder.

server {
listen 80;
server_name myportfolio.com;

    root /var/www/myportfolio/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

}
Important

For React Router, use:

try_files $uri /index.html;

This prevents refresh errors on routes like:

/about
/projects
/contact
Spring Boot API Reverse Proxy

Spring Boot usually runs on port 8080.

server {
listen 80;
server_name api.myapp.com;

    location / {
        proxy_pass http://localhost:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
Node.js App Reverse Proxy

Node apps often run on port 3000.

server {
listen 80;
server_name nodeapp.com;

    location / {
        proxy_pass http://localhost:3000;
    }

}
Serving Frontend and Backend Together

Example:

React frontend served by NGINX
API forwarded to Spring Boot
server {
listen 80;
server_name myapp.com;

    root /var/www/myapp/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:8080/;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

}
HTTPS / SSL Config

Usually done using Certbot.

Install Certbot:

sudo apt install certbot python3-certbot-nginx

Generate SSL certificate:

sudo certbot --nginx -d example.com -d www.example.com

NGINX HTTPS config usually looks like:

server {
listen 443 ssl;
server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    location / {
        proxy_pass http://localhost:8080;
    }

}

Redirect HTTP to HTTPS:

server {
listen 80;
server_name example.com;

    return 301 https://$host$request_uri;

}
Common NGINX Commands
Check NGINX status
sudo systemctl status nginx
Start NGINX
sudo systemctl start nginx
Stop NGINX
sudo systemctl stop nginx
Restart NGINX
sudo systemctl restart nginx
Reload NGINX
sudo systemctl reload nginx

Reload is safer than restart because it applies config changes without fully stopping NGINX.

Test config
sudo nginx -t

Always run this before reloading.

Typical NGINX Workflow
Create or edit config file
sudo nano /etc/nginx/sites-available/myapp
Enable the config
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
Test config
sudo nginx -t
Reload NGINX
sudo systemctl reload nginx
Common Errors
502 Bad Gateway

Means NGINX cannot reach the backend app.

Possible causes:

Backend app is not running
Wrong port in proxy_pass
Firewall issue
App crashed

Check backend:

curl http://localhost:8080
404 Not Found

Means NGINX could not find the requested file or route.

Possible causes:

Wrong root folder
Missing index.html
Wrong try_files config
403 Forbidden

Means NGINX does not have permission to access the files.

Fix permissions:

sudo chmod -R 755 /var/www/myapp
Config test failed

Run:

sudo nginx -t

It will show the file and line number causing the issue.

Important Terms
Reverse Proxy

A server that receives requests and forwards them to another server.

Upstream

A group of backend servers.

upstream app {
server localhost:8080;
}
Load Balancing

Sharing traffic between multiple servers.

SSL Termination

NGINX handles HTTPS, then forwards normal HTTP to the backend.

Static Files

Files like HTML, CSS, JS, and images served directly by NGINX.

Example Full Config
server {
listen 80;
server_name myapp.com www.myapp.com;

    root /var/www/myapp/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:8080/;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

}
Simple Explanation

NGINX sits in front of your app.

Instead of users directly calling:

localhost:8080

They call:

myapp.com

Then NGINX decides what to do:

Serve frontend files
Forward API requests
Redirect HTTP to HTTPS
Balance traffic between servers
Handle SSL certificates
NGINX Mental Model

Think of NGINX as a smart receptionist.

A request comes in:

GET /api/users

NGINX checks the config and says:

/api goes to Spring Boot
/ goes to React
static files come from /var/www
HTTPS should be handled here
invalid routes should return 404

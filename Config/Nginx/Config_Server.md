# 🚀 Nginx configuration

**🌐 A list of parameters to config the Nginx Server 🌐**

## Authentication

### Step 1: Install apache utils

```sh
sudo apt install -y apache2-utils
```

### Step 2: Create passwd file and add users authentication

- Flag c to create file
- Create admin and ttndev users

```sh
htpasswd -c /etc/nginx/passwd admin
htpasswd /etc/nginx/passwd ttndev
```

### Step 3: Add configuration to nginx config file

```sh
server {
    location /secure {
        try_files $uri /secure.html;

        auth_basic "Authentication is required here...";
        auth_basic_user_file /etc/nginx/passwd;

        # Set expired with 1 day
        add_header Cache-Control "public, max-age=3600";
        expires 1d;
    }
}
```

## Add SSL certificate for Nginx Server

### Step 1: Install openssl library

```sh
sudo apt install -y openssl
```

### Step 2: Request SSL certificate
- x509: Generate a self-signed X.509 certificate (A standard format for SSL certificates)
- nodes: No encrypted private key ("No DES" Data Encryption Standard)

```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/openssl/ssl.key -out /etc/nginx/openssl/ssl.cert
```

### Step 3: Add to Nginx configuration file
- Redirect all traffic form HTTP to HTTPS
- Set another port for HTTPS connections
- Add ssl certificate configuration

```sh
# Redirect all traffic form HTTP to HTTPS
server {
    listen 80 default_server;
    server_name localhost;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl default_server;
    ssl_certificate /etc/nginx/openssl/ssl.cert;
    ssl_certificate_key /etc/nginx/openssl/ssl.key;
}
```
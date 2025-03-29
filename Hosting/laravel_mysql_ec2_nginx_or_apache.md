# 🚀 Deploying Laravel on EC2 Ubuntu with Nginx Server and MySQL

**🌐 This guide walks you through the steps to deploy a Laravel application on an Ubuntu server with Nginx 🌐**

---

## Requirements

1. A Laravel application hosted on GitHub.
2. A server running Nginx with SSH access.
3. A domain or subdomain pointing to the server.
4. SSH keys set up on the server for GitHub access ([Reference: SSH Keys and GitHub](https://codewithsusan.com/notes/ssh-keys-and-github)).

---

## Server Preparation

- Ensure the server meets Laravel's requirements ([Laravel Server Requirements](https://laravel.com/docs/deployment#server-requirements)).
- Check PHP version and extensions:
- Ensure installed all PHP extensions requirements by Laravel ([Laravel Deployment Requirement](https://laravel.com/docs/11.x/deployment#main-content)).

  ```bash
  # Check PHP version
  php -v

  # List installed PHP extensions
  php -m
  ```

---

## Set up Ubuntu

To ensure you can install PHP 8.2 on Ubuntu, add the Ondřej Surý PPA, which hosts the latest PHP packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```

---

## Install (Nginx or Apache) and MySQL server

- Install Nginx server:
  ```bash
  sudo apt install nginx -y
  ```
- Install Apache server:
  ```bash
  sudo apt install apache2 -y
  ```
- Install MySQL server:

  ```bash
  sudo apt install mysql-server -y
  ```

- Secure your MySQL installation by running:

  ```bash
  sudo mysql_secure_installation
  ```

- Create a new DB and user:
  ```bash
  sudo mysql -u root -p
  CREATE DATABASE <db_name>;
  CREATE USER '<projectuser>'@'localhost' IDENTIFIED BY '<password>';
  GRANT ALL PRIVILEGES ON <db_name>.* TO <projectuser>@'localhost';
  FLUSH PRIVILEGES;
  EXIT;
  ```

---

## Configure the Firewall

- To allow Apache through the firewall, run:
  ```bash
  sudo ufw allow 'Apache Full'
  ```
- Verify UFW’s status with:
  ```bash
  sudo ufw status
  ```

---

## Install Required PHP Extensions

- Install PHPEnv if needed:
```bash
# Install Required Dependencies
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential libxml2-dev libssl-dev \
    libbz2-dev libcurl4-openssl-dev libjpeg-dev libpng-dev \
    libmcrypt-dev libreadline-dev libtidy-dev libxslt1-dev \
    libzip-dev zlib1g-dev autoconf bison re2c pkg-config

# Clone phpenv Repository
git clone https://github.com/phpenv/phpenv.git ~/.phpenv

echo 'export PHPENV_ROOT="$HOME/.phpenv"' >> ~/.zshrc
echo 'export PATH="$PHPENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(phpenv init -)"' >> ~/.zshrc
source ~/.zshrc
```

- Upgrade PHP if needed:

```bash
sudo apt install php8.2 php8.2-fpm php8.2-xml php8.2-dom php8.2-mysql zip unzip
```

- Enable all services:

```bash
sudo systemctl start php8.2-fpm
sudo systemctl enable php8.2-fpm
```

---

## Install Composer

Composer manages Laravel dependencies.

```bash
cd /usr/bin
curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar composer
composer
```

---

## Clone Your Laravel Application

1. Navigate to the web directory on your server:
   ```bash
   cd /var/www/
   ```
2. Clone your repository:
   ```bash
   git clone git@github.com:user/demo.git
   cd demo
   ```

---

## Install Dependencies

Run Composer to install dependencies:

- For production:
  ```bash
  composer install --optimize-autoloader --no-dev
  ```
- For development:
  ```bash
  composer update
  ```

---

## Configure .env File

Set up the Laravel environment:

```bash
cp .env.example .env
php artisan key:generate
```

Modify the environment variable:

```bash
APP_NAME=LaravelApp
APP_ENV=production
APP_KEY=base64:L/SEAZCKhTNV49LqtPXf7WKvMUQrMVSfRfbKYrveOSE=
APP_DEBUG=false
APP_TIMEZONE=UTC
APP_URL=http://13.229.99.244

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_crud
DB_USERNAME=crud_user
DB_PASSWORD=crud_user
```

Run all command to init application:

```bash
php artisan migrate
php artisan db:seed
```

---

## Set Permissions

Ensure `storage` and `bootstrap/cache` are writable:

```bash
# Get nginx user
ps aux | grep "nginx: worker process" | awk '{print $1}' | grep -v root

# Get apache user
ps aux | grep "apache" | awk '{print $1}' | grep -v root | head -n 1

# Set permissions for necessary directories
sudo chown -R www-data storage
sudo chown -R www-data bootstrap/cache
```

---

## Configure Server

### Nginx server

1. Create an Nginx configuration file in `/etc/nginx/sites-available/demo`:

   ```nginx
   server {
       listen 80;
       listen [::]:80;
       server_name example.com;
       root /srv/example.com/public;

       add_header X-Frame-Options "SAMEORIGIN";
       add_header X-Content-Type-Options "nosniff";

       index index.php;

       charset utf-8;

       location / {
           try_files $uri $uri/ /index.php?$query_string;
       }

       location = /favicon.ico { access_log off; log_not_found off; }
       location = /robots.txt  { access_log off; log_not_found off; }

       error_page 404 /index.php;

       location ~ \.php$ {
           fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
           fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
           include fastcgi_params;
           fastcgi_hide_header X-Powered-By;
       }

       location ~ /\.(?!well-known).* {
           deny all;
       }
     }
   ```

2. Enable the site and test Nginx configuration:

   ```bash
   sudo ln -s /etc/nginx/sites-available/demo /etc/nginx/sites-enabled
   sudo nginx -t
   sudo systemctl restart nginx
   ```

---

### Configure Apache

1. Create an Nginx configuration file in `/etc/nginx/sites-available/demo`:

   ```apache
   <VirtualHost *:80>
    ServerAdmin webmaster@yourdomain.com
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com

    DocumentRoot /var/www/yourdomain.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

2. Enable the site and test Nginx configuration:

   ```bash
   sudo a2ensite yourdomain.com.conf
   apache2ctl -t
   systemctl restart apache2
   ```

3. Enable Apache Modules (Optional):

- mod_rewrite for URL rewriting, often used in SEO and web frameworks.
- mod_ssl for SSL/TLS support.

  ```bash
  sudo a2enmod rewrite
  sudo a2enmod ssl
  ```

4. Security Enhancement (Optional):

- Open Apache’s configuration file:

```bash
   sudo nano /etc/apache2/conf-available/security.conf
```

- Set these two parameters:

  ```bash
  ServerTokens Prod
  ServerSignature Off
  ```

5. Restart Apache server

```bash
  sudo systemctl restart apache2

```

---

## Test Application

Load the app in your browser at `http://yourdomain.com` to verify everything is working.

For troubleshooting, check [Common Laravel Installation Issues](https://codewithsusan.com/notes/common-laravel-installation-issues).

---

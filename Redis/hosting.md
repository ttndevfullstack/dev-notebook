
# Deploying Laravel on Ubuntu Nginx Server

This guide walks you through the steps to deploy a Laravel application on an Ubuntu server with Nginx.

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

    ```bash
    # Check PHP version
    php -v

    # List installed PHP extensions
    php -m
    ```

---

## Install Required PHP Extensions

Upgrade PHP if needed:

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.2 php8.2-xml php8.2-dom php8.2-mysql zip unzip
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
    git clone git@github.com:susanBuck/demo.git
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

---

## Set Permissions

Ensure `storage` and `bootstrap/cache` are writable:

```bash
# Get web server user
ps aux | grep "nginx: worker process" | awk '{print $1}' | grep -v root

# Set permissions for necessary directories
sudo chown -R www-data storage
sudo chown -R www-data bootstrap/cache
```

---

## Configure Nginx

1. Create an Nginx configuration file in `/etc/nginx/sites-available/demo`:

    ```nginx
    server {
        listen 80;
        server_name demo.com;
        root /var/www/demo/public;
        
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-Content-Type-Options "nosniff";
        
        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }
        
        location ~ \.php$ {
            fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
            include fastcgi_params;
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

## Test Application

Load the app in your browser at `http://yourdomain.com` to verify everything is working.

For troubleshooting, check [Common Laravel Installation Issues](https://codewithsusan.com/notes/common-laravel-installation-issues).

---

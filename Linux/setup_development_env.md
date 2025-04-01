# 🚀 Set Up Development Environment on Linux Ubuntu

**🌐 This guide walks you through the steps to set up all dependency for development software in Linux Ubuntu 🌐**

---

## Step to step to setup Development Environment

- Upgrade Ubuntu:
  ```bash
  sudo apt update && sudo apt upgrade -y

  curl --version
  sudo apt install curl
  ```

- Install Nginx server:
  ```bash
  sudo apt install nginx -y
  ```

- Install Apache server:
  ```bash
  sudo apt install apache2 -y
  sudo a2enmod rewrite headers expires
  sudo systemctl restart apache2
  ```

- Install MySQL server
  ```bash
  sudo apt install mysql-server -y
  sudo mysql_secure_installation
  
  # Create a user with full permissions
  sudo mysql -u root -p
  CREATE USER 'ttndev'@'localhost' IDENTIFIED BY 'ttndev';
  GRANT ALL PRIVILEGES ON *.* TO 'ttndev'@'localhost' WITH GRANT OPTION;
  FLUSH PRIVILEGES;

  # Check
  SHOW GRANTS FOR 'ttndev'@'localhost';
  ```

- Install PHPEnv Management
  ```bash
  sudo apt install -y make build-essential libssl-dev libbz2-dev libreadline-dev \
  libsqlite3-dev curl libcurl4-openssl-dev libxml2-dev libxslt1-dev \
  libzip-dev libonig-dev libargon2-dev libtidy-dev libgd-dev \
  libjpeg-dev libpng-dev libfreetype6-dev libgmp-dev libicu-dev \
  libmysqlclient-dev libmcrypt-dev

  # Clone github
  git clone https://github.com/phpenv/phpenv.git ~/.phpenv
  echo '### Load PHPENV' >> ~/.bashrc
  echo 'export PHPENV_ROOT="$HOME/.phpenv"' >> ~/.bashrc
  echo 'export PATH="$PHPENV_ROOT/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(phpenv init -)"' >> ~/.bashrc
  source ~/.bashrc

  # Check
  phpenv --version

  # Clone builder
  git clone https://github.com/php-build/php-build.git ~/.phpenv/plugins/php-build
  ```

- Install PHP Versions
  ```bash
  # Install php dependencies repository 
  sudo apt install software-properties-common -y
  sudo add-apt-repository ppa:ondrej/php -y

  phpenv install --list
  phpenv install <version_number>

  # Set global
  phpenv global 8.2.0

  # Set local project
  phpenv local 8.1.0

  # Check
  php -v

- Update PHP Configure file
  ```bash
  php --ini
  sudo vim <ini_path>

  # Update configuration
  memory_limit = -1
  upload_max_filesize = 64M
  post_max_size = 64M
  max_execution_time = 120

  # Add this lines
  extension=redis.so
  extension=xdebug.so
  extension=imagick.so

  # Apply changed
  phpenv rehash
  ```

- Install Composer and Laravel manager

  ```bash
  # Install PHP (if not already installed)
  sudo apt update && sudo apt install -y php-cli php-zip unzip curl
  
  # Download and install Composer
  curl -sS https://getcomposer.org/installer | php
  
  # Move Composer to global PATH
  sudo mv composer.phar /usr/local/bin/composer
  
  # Verify installation
  composer --version

  # Install Laravel Manager
  composer global require laravel/installer

  echo '### Load laravel installer' >> ~/.bashrc
  echo 'export PATH="$HOME/.composer/vendor/bin:$HOME/.config/composer/vendor/bin:$PATH"' >> ~/.bashrc
  
  # Check
  laravel
  ```

- Install Node Version Manager
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

  # Add to ~/.zshrc
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
  
  # Apply
  source ~/.zshrc

  # Check
  nvm --version
  npm --version

  nvm install <version_number>
  npm install -g yarn pnpm
  ```

- Install Docker, Lazy Docker
  ```bash
  # Add Docker's official GPG key:
  sudo apt-get update
  sudo apt-get install ca-certificates curl
  sudo install -m 0755 -d /etc/apt/keyrings
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
  sudo chmod a+r /etc/apt/keyrings/docker.asc

  # Add the repository to Apt sources:
  echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update

  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

  # Install Lazy Docker
  curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
  ```

- Configure Git User
  ```bash
  git init
  git config --global user.name "i-am_truongtrungnghia"
  git config --global user.email "your.email@example.com"
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

### Configure Nginx server

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

### Configure Apache server

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

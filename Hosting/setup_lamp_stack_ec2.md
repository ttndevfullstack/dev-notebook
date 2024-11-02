# Hosting the Nail Booking System on AWS EC2

### Step 1: Allow access network in ec2 instance
- Under Security Group, allow the following:
  - **SSH (Port 22)** – for secure shell access.
  - **HTTP (Port 80)** – for web access.
  - **HTTPS (Port 443)** – for secure web access.

### Step 2: Connect to EC2 Instance
- Using SSH to connect

```sh
ssh -i "your-key-file.pem" ubuntu@<public_ip>
sudo apt update && sudo apt upgrade -y
```

### Step 1: Check php version and necessary packages
```sh
cat composer.json | grep '"php":'
composer check-platform-reqs
```

### Step 2: Install PHP, Apache, Mysql Server, neccesary packages 
```sh
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2

sudo apt install -y php8.1 libapache2-mod-php8.1 php8.1-cli php8.1-common php8.1-mbstring php8.1-xml php8.1-curl php8.1-zip php8.1-bcmath php8.1-mysql
sudo systemctl restart apache2

sudo apt install mysql-server -y
sudo mysql_secure_installation
sudo mysql -u root -p
CREATE DATABASE <db_name>;
CREATE USER '<projectuser>'@'localhost' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON <db_name>.* TO <projectuser>@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
- Import base data to db
```sh
mysql -u <projectuser> -p <db_name> < /file.sql
```

### Step 3: Setup Apache
```sh
sudo vim /etc/apache2/sites-available/<project>.conf

<VirtualHost *:80>
    ServerName 13.213.38.159
    ServerAlias 13.213.38.159
    DocumentRoot /var/www/<project>/public/

    <Directory /var/www/<project>/public/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/<project>_error.log
    CustomLog ${APACHE_LOG_DIR}/<project>_access.log combined
</VirtualHost>

sudo a2ensite <project>
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### Step 4: Setup Project
- Clone or move project to /var/www/
- Install & setup all thing

### Step 3: Set files permissions
+ Grant owner for group and user of Apache (www-data:www-data)
+ Grant 755 for /var/www path (7: owner full, 5: group read & execute, 5: anyone read & execute)
```sh
sudo chown -R www-data:www-data /var/www
sudo chmod -R 755 storage
sudo chmod -R 755 bootstrap/cache
```
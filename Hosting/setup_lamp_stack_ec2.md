# Hosting the Nail Booking System on AWS EC2

To host a project application from GitHub on a free AWS EC2 instance in Singapore using the LAMP (Linux, Apache, MySQL, PHP) stack, follow these step-by-step instructions. This process includes deploying an Amazon EC2 instance, setting up the LAMP stack, configuring your GitHub repository, and ensuring the website is available on the network.

## 1. Create an AWS EC2 Instance in Singapore (Free Tier)

### Step 1: Launch EC2 Instance
1. Log in to your AWS Management Console.
2. Navigate to the EC2 Dashboard.
3. Click **Launch Instance**.
4. Name the instance (e.g., NailBookingSystem).
5. Select **Ubuntu Server 20.04 LTS (64-bit)**.
6. Under instance type, select **t2.micro** (this is covered under the AWS free tier)..
7. Choose the **region** Singapore

### Step 2: Configure Instance Details
- Leave the defaults for Network and Subnet unless you have a custom setup.
- Under Security Group, allow the following:
  - **SSH (Port 22)** – for secure shell access.
  - **HTTP (Port 80)** – for web access.
  - **HTTPS (Port 443)** – for secure web access.
- Proceed with **Review and Launch**.

### Step 3: Launch the Instance
- Create or use an existing SSH key pair to access the instance.
- Launch the instance.

### Step 4: Connect to EC2 Instance
After the instance is launched, click on **Connect** and follow the instructions to SSH into your instance from the terminal.

```sh
ssh -i "your-key-file.pem" ubuntu@your-ec2-public-ip
```
Replace `your-key-file.pem` with the path to your SSH key and `your-ec2-public-ip` with the public IP of the instance.

## 2. Set Up the LAMP Stack

### Step 1: Update Package Repositories
```sh
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Apache
```sh
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```
You can verify that Apache is running by visiting `http://your-ec2-public-ip` in your browser.

### Step 3: Install MySQL
```sh
sudo apt update
sudo apt install mysql-server -y
sudo mysql_secure_installation
```
Follow the prompts to secure your MySQL installation (set a root password, remove anonymous users, disallow root login remotely, etc.).

### Step 4: Install PHP
```sh
sudo apt update
sudo apt install php libapache2-mod-php php-mysql -y
sudo systemctl restart apache2
```

## 3. Clone Your GitHub Repository

### Step 1: Install Git
```sh
sudo apt install git -y
```

### Step 2: Clone the GitHub Repository
Copy ssh from local machine to ec2 instance:
```sh
scp -i ~/.ssh/aws_instances_singapore_key_pair.pem ~/.ssh/* ubuntu@<public_ip>:/home/ubuntu/.ssh/
```
Navigate to Apache's default web directory and clone your repository:
```sh
cd /var/www/html
sudo rm index.html  # Remove default Apache file
sudo git clone https://github.com/ttndevfullstack/nail_booking_system_mutiple_business.git
```

### Step 3: Set Proper Permissions
+ Grant owner for group and user of Apache (www-data:www-data)
+ Grant 755 for /var/www/html path (7: owner full, 5: group read & execute, 5: anyone read & execute)
```sh
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

## 4. Configure Apache for Your Project

### Step 1: Set Up Virtual Host
Create a new Apache configuration file for your site:
```sh
sudo vim /etc/apache2/sites-available/nail_booking.conf
```
Add the following configuration:
```apache
<VirtualHost *:80>
    ServerAdmin admin@example.com   # Apache will notify to this email
    DocumentRoot /var/www/html
    ServerName your-domain.com  # Or Public Address IP

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Save and exit (Ctrl + O, Enter, Ctrl + X).

### Step 2: Enable Virtual Host and Rewrite Module
```sh
sudo a2ensite nail_booking.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
sudo systemctl restart apache2
```

## 5. Database Setup (MySQL)

### Step 1: Create MySQL Database
Log into MySQL:
```sh
sudo mysql -u root -p
```
Create a new database for the application:
```sql
CREATE DATABASE nail_booking_db;
CREATE USER 'nailuser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON nail_booking_db.* TO 'nailuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 2: Import Your Database
If your GitHub repo includes an SQL file:
```sh
mysql -u nailuser -p nail_booking_db < /path-to-your-database.sql
```

## 6. Configure the Application

### Step 1: Config Application
+ Config environment file
```sh
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:0hlb+oA9IM1na+6qYKeaNo5qxRHpkwx2Mhkvs4+H5Gc=
APP_DEBUG=true
APP_URL=http://122.248.225.142
APP_ADMIN_DOMAIN=admin.122.248.225.142
APP_OWNER_DOMAIN=122.248.225.142
APP_DOMAIN=122.248.225.142
APP_LINK=nail://customer

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name_here
DB_USERNAME=your_database_user_here
DB_PASSWORD=your_database_password_here
```
+ Install Composer, NPM, Node, NPX
```sh
cd ~
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo apt update
sudo apt install nodejs npm -y
sudo npm install -g npx
```

+ Install package
```sh
cd /var/www/html/nail_booking_system
composer install --no-dev
npm install
npm run production
```

+ Setup larvel app
```sh
php artisan key:generate
```

+ Grant permission for storage * cache
```sh
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html/storage
sudo chmod -R 755 /var/www/html/bootstrap/cache
```

### Step 2: Set Permissions and Restart Apache
```sh
sudo chown -R www-data:www-data /var/www/html
sudo systemctl restart apache2
```

## 7. Testing and Access
Visit your site at `http://your-ec2-public-ip`. If you set up everything correctly, your application should be running.

You can also map a custom domain to your EC2 instance by configuring your DNS settings in your domain registrar and pointing the A record to your instance's public IP.

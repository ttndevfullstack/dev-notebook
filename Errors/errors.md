# 🚀 Error notebooks

**🌐 List errors in software development and solutions to fix 🌐**

### ❌ The csv file failed to upload
+ 🔴 Reason: File is uploaded too large
+ 🛠️ Solution: Update php.ini file to accept large files

```bash
upload_max_filesize = 100M
post_max_size = 100M
max_execution_time = 300
max_input_time = 300

sudo systemctl restart apache2
sudo systemctl restart nginx
```

### ❌ Class "ZipArchive" not found
+ 🔴 Reason: Missing php extension
+ 🛠️ Solution: Install the php-zip extension

```bash
sudo apt install php-zip
```

### ❌ Class "DOMDocument" not found
+ 🔴 Reason: Missing php extension
+ 🛠️ Solution: Install the php-dom extension

```bash
sudo apt install php-dom
```

### ❌ PHP Warning: PHP Startup: Unable to load dynamic library 'pdo_sqlite' (tried: /usr/lib/php/20220829/pdo_sqlite (/usr/lib/php/20220829/pdo_sqlite: cannot open shared object file: No such file or directory), /usr/lib/php/20220829/pdo_sqlite.so (/usr/lib/php/20220829/pdo_sqlite.so: undefined symbol: php_pdo_unregister_driver)) in Unknown on line 0 PHP Warning: Module "sqlite3" is already loaded in Unknown on line 0 The environment file is invalid! Failed to parse dotenv file. Encountered unexpected whitespace at [Nail Booking].
+ 🔴 Reason: Error path to load pdo_sqlite extention
+ 🛠️ Solution: Reinstall pdo_sqlite extention

1. Command pdo_sqlite and sqlite3 extension in php.ini file
2. Reinstall
```bash
sudo apt install --reistall pdo_sqlite
```



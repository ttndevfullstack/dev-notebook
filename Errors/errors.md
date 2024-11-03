# 🚀 Error notebooks

**List errors in software development and solutions to fix**

## The csv file failed to upload.
### Reason: File is uploaded too large
### Solution: Update php.ini file to accept large files

```bash
upload_max_filesize = 100M
post_max_size = 100M
max_execution_time = 300
max_input_time = 300

sudo systemctl restart apache2
sudo systemctl restart nginx
```



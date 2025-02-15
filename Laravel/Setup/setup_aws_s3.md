# 🚀 Set Up AWS S3

**🌐 Steps to Step to set up AWS S3 in Laravel Project 🌐**

---

1. Install dependencies:
```bash
composer require league/flysystem-aws-s3-v3 "^3.0"
```

2. Configure environment:
Get AWS keys
```bash
cat ~/.aws/credentials
```

```env
AWS_ACCESS_KEY_ID=your-access-key-id
AWS_SECRET_ACCESS_KEY=your-secret-access-key
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=your-bucket-name
AWS_URL=https://your-bucket-name.s3.amazonaws.com
```

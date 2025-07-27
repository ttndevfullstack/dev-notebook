# 🚀 Create A AWS S3 Bucket Via Terminal

**🌐 Create a AWS S3 Bucket via terminal with full configuration 🌐**

---

1. Create the bucket:
```bash
aws s3api create-bucket \
  --bucket plagiarism-checker-system \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
```

2. Enable versioning:
```bash
aws s3api put-bucket-versioning \
  --bucket plagiarism-checker-system \
  --versioning-configuration Status=Enabled

# Check bucket versioning
aws s3api get-bucket-versioning --bucket plagiarism_checker_system
```

3. Enable versioning:
Key Benefits of S3 Versioning
✅ Every upload creates a new version
✅ Soft Delete
✅ Restore Previous Versions

Potential Downsides
❌ Increases storage costs (since old versions are retained)
❌ Needs version management (you may need lifecycle rules to clean up old versions)

```bash
aws s3api put-bucket-versioning \
  --bucket plagiarism-checker-system \
  --versioning-configuration Status=Enabled

# Check
aws s3api get-bucket-versioning --bucket plagiarism-checker-system
```

4. Enable server-side encryption (AES256):
Key Benefits of S3 Versioning
✅ Encryption data
✅ Meet multiple compliance requirements (GDPR, HIPAA, PCI DSS)
✅ KMS (Key Management Service) provides logging and auditing capabilities to track

Potential Downsides
❌ Encryption can introduce latency
❌ Increased Costs for Key Management (SSE-KMS)
❌ Complexity

```bash
aws s3api put-bucket-encryption \
  --bucket plagiarism_checker_system \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }]
  }'

# Check bucket encryption
aws s3api get-bucket-encryption --bucket plagiarism-checker-system
```

4. Enable server-side encryption (AES256):
Key Benefits of S3 Versioning
✅ Encryption data
✅ Meet multiple compliance requirements (GDPR, HIPAA, PCI DSS)
✅ KMS (Key Management Service) provides logging and auditing capabilities to track

Potential Downsides
❌ Encryption can introduce latency
❌ Increased Costs for Key Management (SSE-KMS)
❌ Complexity

```bash
aws s3api put-bucket-policy \
  --bucket plagiarism_checker_system \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "AllowLocalAccess",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:*",
        "Resource": [
          "arn:aws:s3:::plagiarism_checker_system",
          "arn:aws:s3:::plagiarism_checker_system/*"
        ],
        "Condition": {
          "IpAddress": {
            "aws:SourceIp": "<ACCEPTED_IP_ADDRESS>"
          }
        }
      }
    ]
  }'

# Check bucket policy
aws s3api get-bucket-policy --bucket plagiarism_checker_system
```

5. Enable cors:
Create a configuration json file
```json
{
  "CORSRules": [
      {
          "AllowedOrigins": ["*"],
          "AllowedMethods": ["GET", "POST", "PUT", "DELETE"],
          "AllowedHeaders": ["*"],
          "ExposeHeaders": ["ETag"],
          "MaxAgeSeconds": 3000
      }
  ]
}
```

Run this command to enable cors
```bash
aws s3api put-bucket-cors --bucket plagiarism-checker-system --cors-configuration file://../resource/AWS/config/s3-bucket-cors.json

# Verify cors configuration
aws s3api get-bucket-cors --bucket plagiarism-checker-system
```


# CloudLaunch – AWS Project Submission

## Project Description

This is the AltSchool of Engineering Semester 3 Assignment – a two-part project deploying cloud infrastructure using Amazon Web Services (AWS):

- **Task 1:** S3 buckets, website hosting, IAM policy and user
- **Task 2:** Custom VPC with subnets, route tables, security groups

---

## 🧾 Task 1: S3 Buckets + IAM

- **cloudlaunch-site-bucket** – Static website hosting (public, read-only)
- **cloudlaunch-private-bucket** – Internal document storage (private with IAM-controlled access)
- **cloudlaunch-visible-only-bucket** – Listable only, no object access

**Static Website URL:** [Visit Site](https://firstcloudlaunch-site-bucket.s3.eu-north-1.amazonaws.com/cloudwatchpage.html)

**CloudFront URL:** [d13roytp0ptn1c.cloudfront.net]

**IAM Policy JSON:**

## 🔐 Task 2 – IAM User + Private Buckets

### S3 Buckets Setup:

| Bucket Name                        | Type               | Permissions                       |
|------------------------------------|--------------------|------------------------------------|
|`firstcloudlaunch-site-bucket`          | Public Static Site | GetObject only                     |
| `secondcloudlaunch-private-bucket`       | Private             | GetObject, PutObject               |
| `thirdcloudlaunch-visible-only-bucket`  | Visible-only        | ListBucket only                    |

### IAM User Configuration:

- 👤 User: `cloudlaunch-user`
- 👥 Group: `cloudlaunch-admin`
- 📜 Policy: `cloudlaunch-userjsonpolicy`

---

## 🔑 IAM Policy Attached to cloudlaunch-user

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListAllBuckets",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": [
                "arn:aws:s3:::cloudlaunch-site-bucket",
                "arn:aws:s3:::cloudlaunch-private-bucket",
                "arn:aws:s3:::cloudlaunch-visible-only-bucket"
            ]
        },
        {
            "Sid": "AllowReadWritePrivateBucket",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-private-bucket/*"
        },
        {
            "Sid": "AllowReadOnlySiteBucket",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloudlaunch-site-bucket/*"
        }
    ]
}
```
📄 [Download cloudlaunch-user-policy.json](./cloudlaunch-user-policy.json)
### 🔐 Second Policy: `SecondCloudLaunchPrivateAccessPolicy`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::secondcloudlaunch-private-bucket/*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::secondcloudlaunch-private-bucket"
        }
    ]
}
```

📄 [Download SecondCloudLaunchPrivateAccessPolicy.json](./SecondCloudLaunchPrivateAccessPolicy.json)
---

## 📌 Notes

- All policies follow least privilege principle.
- DeleteObject permissions were intentionally excluded.
- Static site tested and publicly viewable.
- Bonus CloudFront distribution provides global caching and HTTPS access.

---






## 🕸️ Task 2: VPC Design

- **VPC CIDR:** `10.0.0.0/16`

### Subnets
- `10.0.1.0/24` – Public Subnet
- `10.0.2.0/24` – App Subnet
- `10.0.3.0/28` – DB Subnet

### Route Tables
- Public subnet route table connected to Internet Gateway
- App & DB subnets with isolated routes (no internet)

### Security Groups
- **cloudlaunch-app-sg:** HTTP (port 80) from inside VPC only
- **cloudlaunch-db-sg:** MySQL (port 3306) from app subnet only

---

## ✅ Extras

- **Account ID / Console Alias:** `807267567931`  
**User login link:** [AWS Console Login](https://807267567931.signin.aws.amazon.com/console)  
**Password Reset:** ✅ Enforced on first login  
**Username
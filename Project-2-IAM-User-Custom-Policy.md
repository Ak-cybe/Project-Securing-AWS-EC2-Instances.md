
# 🛡️ Project 2: Implementing Least Privilege with AWS IAM

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazon-aws)
![IAM](https://img.shields.io/badge/Security-IAM-red?style=for-the-badge&logo=security)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

## 📖 Project Overview

This project demonstrates the implementation of the **Principle of Least Privilege** in AWS. The goal was to move away from using root accounts or full-access admin users by creating a specialized IAM user with strictly limited permissions.

The project involves creating a custom JSON policy that restricts a user to **only** read and list files from a specific S3 bucket, denying all other actions.

---

## 🎯 Objectives

- **Identity Management:** Create a dedicated programmatic user (`s3-read-user`)
- **Access Control:** Draft a custom IAM policy using JSON to define granular permissions
- **Verification:** Authenticate and test permissions using the AWS Command Line Interface (CLI)

---

## 🛠️ Step-by-Step Implementation

### Phase 1: IAM User Configuration

1. Log in to the **AWS Management Console** as an Administrator
2. Navigate to **IAM Dashboard** → **Users** → **Add user**
3. **User Details:**
   - **Username:** `s3-read-user`
   - **Access Type:** Programmatic Access (Generates Access Key ID & Secret Access Key)

---

### Phase 2: Defining the Security Policy (JSON)

Instead of attaching a managed policy like `AmazonS3ReadOnlyAccess` (which grants access to *all* buckets), I created an **Inline Policy** to restrict access to a specific resource.

**Policy Definition:**

{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "AllowS3ReadAccess",
"Effect": "Allow",
"Action": [
"s3:ListBucket",
"s3:GetObject"
],
"Resource": [
"arn:aws:s3:::my-secure-bucket",
"arn:aws:s3:::my-secure-bucket/*"
]
}
]
}



#### 🔍 Technical Breakdown:

| Component | Purpose |
|-----------|---------|
| `s3:ListBucket` | Allows listing objects inside the bucket (applied to the bucket ARN) |
| `s3:GetObject` | Allows downloading/reading files (applied to objects `/*`) |
| Resource Restriction | Policy is locked to `my-secure-bucket`, preventing access to any other data |

---

### Phase 3: CLI Configuration & Testing

After generating the credentials, I configured the local environment to simulate a developer accessing the cloud resources.

**1. Configure AWS CLI:**

aws configure --profile s3-user
AWS Access Key ID: [Paste Key ID]
AWS Secret Access Key: [Paste Secret Key]
Default region name: us-east-1
Default output format: json



**2. Verify Access (Success Scenario):**

aws s3 ls s3://my-secure-bucket --profile s3-user



✅ **Result:** Successfully listed files

**3. Verify Access (Failure Scenario - Security Test):**

I attempted to list a different bucket to ensure the policy works:

aws s3 ls s3://other-sensitive-bucket --profile s3-user



❌ **Result:** Access Denied (As expected)

---

## 🚀 Use Case Scenarios

This configuration is ideal for:

- **Third-party Applications:** Giving an external reporting tool access to read logs from one specific bucket
- **Developers:** Allowing a frontend developer to fetch assets without admin rights to the entire AWS account
- **Microservices:** Services that only need to read configuration files
- **CI/CD Pipelines:** Automated deployment processes requiring read access to artifact buckets

---

## 🔐 Security Best Practices Implemented

| Practice | Description |
|----------|-------------|
| **Least Privilege** | User has 0 permissions by default; only explicit allowances are added |
| **Resource Constraints** | Policy is restricted to specific ARNs, not `*` (all resources) |
| **Credential Safety** | Access Keys are not hardcoded in scripts; used via AWS CLI profiles |
| **Separation of Duties** | Created a specific user instead of sharing Admin credentials |
| **Regular Auditing** | Use CloudTrail to monitor IAM user activity |

---

## ⚠️ Important Security Notes

> **Never commit AWS credentials to version control!**

Always use:
- Environment variables
- AWS CLI profiles
- IAM Roles (preferred for EC2/Lambda)
- AWS Secrets Manager for production

---

## 📊 Learning Outcomes

After completing this project, you will understand:

- ✅ How to create IAM users with programmatic access
- ✅ Writing custom JSON policies for fine-grained control
- ✅ The difference between inline and managed policies
- ✅ How to test IAM permissions using AWS CLI
- ✅ Implementing the Principle of Least Privilege

---

## 🔄 Future Improvements

- [ ] **MFA Enforcement:** Add a condition in the policy to require Multi-Factor Authentication
- [ ] **IAM Roles:** Transition from IAM Users to IAM Roles for EC2 integration
- [ ] **Policy Versioning:** Implement policy version control using AWS Policy Simulator
- [ ] **CloudWatch Alarms:** Set up alerts for unauthorized access attempts
- [ ] **Session Policies:** Implement temporary credentials with session policies

---

## 📚 Resources & References

- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [S3 Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html)
- [AWS Policy Simulator](https://policysim.aws.amazon.com/)
- [OWASP Cloud Security](https://owasp.org/www-project-cloud-security/)

---

## 👤 Author

**[Your Name]**
- GitHub: [@Ak-cybe](https://github.com/Ak-cybe)
- LinkedIn: [amrresh kumar](https://www.linkedin.com/in/amresh-kumar-7b5ab8326/)
- Portfolio: [Your Website]

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🌟 Acknowledgments

- AWS Documentation Team
- Cloud Security Community
- [Add any mentors or resources that helped]

---

**⭐ If this project helped you understand IAM policies, please star the repository!**

---

<p align="center">Made with ❤️ for the Cloud Security Community</p>

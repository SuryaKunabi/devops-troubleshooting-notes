
# AWS Troubleshooting – Real Issues & Solutions

This document records AWS CLI, EC2, Terraform, and cloud configuration issues encountered during hands-on AWS practice.

## 1. AWS CLI – Region Configuration

**Problem:**

AWS CLI region configuration needed to be configured before executing AWS commands.

**Troubleshooting Commands:**

```bash
aws --version
aws configure
aws configure list
aws configure get region
```

**Solution:**

Configure the AWS access credentials, default region, and output format.

```bash
aws configure
```

Enter the appropriate:
- AWS Access Key ID
- AWS Secret Access Key
- Default region name
- Default output format

**Learning:** AWS CLI uses configured credentials and region settings when executing commands.

---

## 2. AWS – eksctl Command Not Recognized on Windows

**Problem:**

eksctl was downloaded, but Windows Command Prompt could not recognize the command.

**Actual Error:**

```text
C:\Users\Surya>eksctl.exe version

'eksctl.exe' is not recognized as an internal
or external command,
operable program or batch file.
```

However, executing the binary through its full path worked:

```cmd
"C:\Users\Surya\eksctl_Windows_amd64\eksctl.exe" version
```

**Output:**

```text
0.232.0
```

**Root Cause:**

The directory containing `eksctl.exe` was not added to the Windows PATH environment variable.

**Solution:**

Add the following directory to the Windows PATH:

```text
C:\Users\Surya\eksctl_Windows_amd64
```

Restart Command Prompt and verify:

```cmd
eksctl version
```

**Learning:** Downloading a CLI executable does not automatically make it available globally. Configure PATH correctly.

---

## 3. Terraform AWS Provider – Download Issue

**Problem:**

Terraform encountered a provider download issue while working with AWS provider version `6.64.0`.

**Environment:**

- Terraform
- AWS Provider
- Windows
- AWS infrastructure provisioning

**Commands Used:**

```bash
terraform init
terraform validate
terraform plan
```

**Troubleshooting:**

1. Checked Terraform configuration.
2. Reviewed the provider version requirement.
3. Investigated provider initialization and download failures.

**Solution / Verification:**

Check internet connectivity, Terraform Registry access, provider constraints, and the initialization error details before retrying.

```bash
terraform init -upgrade
terraform providers
```

**Learning:** Terraform provider initialization depends on successfully downloading compatible provider plugins.

---

## 4. AWS – Terraform Remote State Configuration

**Problem:**

While practising Terraform remote state, S3 backend and DynamoDB-based state locking configuration required additional setup.

**Components:**

- Amazon S3
- DynamoDB
- Terraform
- AWS IAM

**Troubleshooting Checklist:**

1. Verify the S3 bucket exists.
2. Verify the configured state key.
3. Check IAM permissions.
4. Verify backend region.
5. Check DynamoDB table configuration if using the locking mechanism.

**Commands:**

```bash
terraform init
terraform plan
terraform state list
```

**Learning:** Remote state requires correct backend configuration and AWS permissions.

**Note:** DynamoDB-based locking is a legacy approach in newer Terraform versions; verify the locking method supported by your Terraform version.

---

## Key Learnings

- Configure AWS CLI correctly.
- Understand Windows PATH configuration.
- Troubleshoot AWS provider initialization.
- Understand Terraform remote state.
- Verify IAM permissions and AWS region settings.

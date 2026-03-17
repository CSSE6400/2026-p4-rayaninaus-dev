# CSSE6400 Week 4 Practical

This project deploys the static website [Hextris](https://hextris.io/) to an AWS EC2 instance using Terraform.

The practical covers:
- manually deploying Hextris through the AWS Console
- redeploying the same application using Infrastructure as Code (Terraform)
- configuring an EC2 instance, security group, startup script, and Terraform output

## Project Files

- `main.tf`
  Terraform configuration for the AWS provider, EC2 instance, security group, and output URL.

- `serve-hextris.sh`
  Startup script used by EC2 `user_data` to install `httpd`, install `git`, and clone the Hextris repository into `/var/www/html`.

- `credentials`
  Local AWS credentials file used by Terraform. This file must not be committed.

## Requirements

Before running this project, make sure you have:

- Terraform installed
- access to AWS Academy Learner Lab
- a valid `credentials` file copied from **AWS Details -> AWS CLI**
- Python 3.12 installed
- Pipenv installed for running the provided local tests

## How to Run

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd 2026-p4-rayaninaus-dev
```

### 2. Create the AWS credentials file

Create a file named `credentials` in the project root and paste in the AWS CLI credentials from Learner Lab.

Example format:

```ini
[default]
aws_access_key_id=YOUR_ACCESS_KEY
aws_secret_access_key=YOUR_SECRET_KEY
aws_session_token=YOUR_SESSION_TOKEN
```

Do not commit this file.

### 3. Initialise Terraform

```bash
terraform init
```

### 4. Review the execution plan

```bash
terraform plan
```

### 5. Deploy the infrastructure

```bash
terraform apply
```

Type `yes` when prompted.

### 6. Get the deployed website URL

```bash
terraform output -raw hextris-url
```

Open the returned URL in a browser to access Hextris.

## How It Works

The Terraform configuration does the following:

- configures the AWS provider in `us-east-1`
- creates a security group allowing:
  - HTTP on port 80
  - SSH on port 22
- creates an EC2 instance using the Amazon Linux 2023 AMI
- uses `user_data` to run `serve-hextris.sh` during instance startup
- outputs the public website URL after deployment

## Running Tests

Local tests can be run with:

```bash
.csse6400/bin/unittest.sh
```

## Destroying the Infrastructure

To remove the deployed AWS resources:

```bash
terraform destroy
```

Type `yes` when prompted.

This should be done after finishing the practical to avoid consuming Learner Lab credit.

## Notes

- The `credentials` file is ignored by Git and should never be committed.
- The public IP address will usually change after running `terraform destroy` and `terraform apply` again.
- The website is served over HTTP, not HTTPS.

## Practical Outcome

This practical demonstrates how Infrastructure as Code can be used to reproducibly provision and redeploy a simple web application on AWS.



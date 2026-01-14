# AWS Infrastructure Deployment

This project enables automatic deployment of AWS infrastructure (VPC, EC2, S3) via GitHub Actions and Terraform.

## 📋 Prerequisites

- An AWS account with necessary permissions
- AWS credentials configured in GitHub secrets
- Terraform 1.5.2 (automatically installed by the pipeline)

## 🔑 GitHub Secrets Configuration

Before using the pipeline, you must configure the following secrets in your GitHub repository:

1. Go to **Settings** > **Secrets and variables** > **Actions**
2. Add the following secrets:
   - `AWS_ACCESS_KEY_ID`: Your AWS access key
   - `AWS_SECRET_ACCESS_KEY`: Your AWS secret key

## 🚀 Usage

### Manual Pipeline Trigger

1. Navigate to the **Actions** tab in your repository
2. Select the **Deploy Infrastructure** workflow
3. Click **Run workflow**
4. Fill in the following parameters:

| Parameter           | Description                   | Default Value          |
| ------------------- | ----------------------------- | ---------------------- |
| `AWS_REGION`        | AWS region for deployment     | `eu-west-3`            |
| `NAME_PREFIX`       | Prefix for naming resources   | `ec2`                  |
| `EC2_INSTANCE_TYPE` | EC2 instance type             | `t2.nano`              |
| `PUBLIC_SSH_KEY`    | Public SSH key for connection | `ssh-rsa AAAAB3Nza...` |

5. Click **Run workflow** to start the deployment

## 📦 Deployed Resources

The pipeline automatically creates the following resources:

- **S3 Bucket**: For storing Terraform state
- **VPC**: Virtual Private Cloud
- **EC2 Instance**: Instance configured with your SSH key
- Associated network resources (subnets, security groups, etc.)

## 🔄 Pipeline Steps

1. **Checkout**: Retrieve repository code
2. **Setup Terraform**: Install Terraform 1.5.2
3. **Setup AWS CLI**: Install AWS CLI
4. **Name Control**: Format GitHub username
5. **S3 Check**: Create S3 bucket for Terraform state if needed
6. **Terraform Init**: Initialize Terraform with S3 backend
7. **Terraform Validate**: Validate configuration
8. **Terraform Apply**: Deploy infrastructure

## 📁 Project Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy-infrastructure.yml
├── scripts/
│   ├── github_actor_name_control.sh
│   └── check_s3_bucket.sh
├── terraform/
│   └── aws_infra/
│       └── (Terraform files)
└── README.md
```

## 🔐 Connecting to EC2 Instance

Once deployment is complete, you can connect to your EC2 instance via SSH:

```bash
ssh -i /path/to/your/private/key ec2-user@<instance-public-ip>
```

The instance's public IP will be displayed in the Terraform outputs.

## ⚠️ Important Notes

- The S3 bucket for Terraform state is automatically created with the name: `<NAME_PREFIX>-<ACTOR_PREFIX>-s3-tfstate`
- Make sure to replace the default public SSH key with your own
- Deployed AWS resources may incur costs depending on your usage

## 🛠️ Customization

To customize the deployed infrastructure, modify the Terraform files in the `terraform/aws_infra/` folder.

## 👥 Contributing

Contributions are welcome! Feel free to open an issue or pull request.
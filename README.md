# aws-architecture
Architecture Diagrams &amp; Clouformation Scripts for One-Click Deployments


# Portfolio Website Infrastructure - CloudFormation

This CloudFormation template sets up the infrastructure for hosting a portfolio website on AWS. It provisions a VPC, a public subnet, an EC2 instance with a public IP, and necessary networking resources like an internet gateway and a security group.

## Implementation Details
- Creates a **VPC** with CIDR `10.0.0.0/16`.
- Sets up a **public subnet** in `us-east-1a`.
- Provisions an **Internet Gateway** and attaches it to the VPC.
- Configures a **route table** to enable internet access for the subnet.
- Defines a **Security Group** allowing HTTP (80), HTTPS (443), and SSH (22) access.
- Deploys an **EC2 instance** using a user-specified AMI and key pair.
- Allocates and associates an **Elastic IP (EIP)** to the EC2 instance.

## Prerequisites
Before deploying this template, ensure you have:
1. **A Custom AMI** – The AMI should have a pre-configured web server and required files. If not, you can install them manually after instance creation.
2. **An EC2 Key Pair** – Create an AWS key pair to enable SSH access. You must specify its name when deploying the template.

## Parameters
When deploying the CloudFormation stack, you need to provide:

| Parameter  | Description |
|------------|-------------|
| `ImageId`  | The Amazon Machine Image (AMI) ID for the EC2 instance. Must be a valid AMI in your AWS region. |
| `KeyName`  | The name of an existing EC2 key pair for SSH access. |

## Deployment Instructions
### Using AWS CLI:
```sh
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name PortfolioStack \
  --parameter-overrides ImageId=ami-12345678 KeyName=my-keypair
```
Replace `ami-12345678` with your actual AMI ID and `my-keypair` with your key pair name.

### Using AWS Console:
1. Navigate to **CloudFormation** in AWS Management Console.
2. Click **Create stack** → **With new resources**.
3. Upload the template file.
4. Specify values for `ImageId` and `KeyName`.
5. Click **Next** and follow the prompts to create the stack.

## Post-Deployment Steps
If the AMI is not pre-configured with a web server, SSH into the instance and set it up manually:
```sh
ssh -i my-keypair.pem ec2-user@<PublicIP>
```
Then install a web server (e.g., Apache or Nginx):
```sh
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```
Deploy your website files in `/var/www/html/`.

## Outputs
After deployment, CloudFormation will output the **Public IP** of the web server. You can access your website via `http://<PublicIP>`.

---
### Notes
- Ensure the security group allows inbound traffic for HTTP, HTTPS, and SSH.
- The instance type is set to `t2.micro`, and the entire deployment can be run in the free tier (tested with a fresh account at the time of writing). However, always remember to monitor the rersources provisioned, costs, and latest AWS documentation).

Happy deploying! 🚀


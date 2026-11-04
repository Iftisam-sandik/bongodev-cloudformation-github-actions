# Homework Submission Report

**Homework:** AWS CloudFormation with GitHub Actions  
**AWS Region:** `ap-south-1` (Mumbai)  
**GitHub Repository:** [bongodev-cloudformation-github-actions](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions)

## 1. Project Summary

For this homework, I deployed an Ubuntu 24.04 EC2 instance with Nginx using AWS CloudFormation and GitHub Actions. I used the provided CloudFormation template and workflow, then configured GitHub, IAM, and OIDC to automate the deployment.

The first Git push created the AWS infrastructure, and the second push updated the existing EC2 `Course` tag. Both deployments completed successfully.

## 2. GitHub Actions, CloudFormation and OIDC

**GitHub Actions vs CloudFormation:** GitHub Actions ran the deployment steps automatically after I pushed the CloudFormation template to the `main` branch. CloudFormation used the YAML template to create the AWS resources and later update them. I used GitHub Actions to automate the work, and CloudFormation to manage the infrastructure itself.

In this project, GitHub Actions first checked out the repository, configured AWS credentials, and validated the CloudFormation template before starting the deployment. CloudFormation then created the VPC, subnet, security group, and EC2 instance based on the template. When I changed the EC2 `Course` tag and pushed the file again, GitHub Actions ran the same workflow, while CloudFormation updated the existing stack. This helped me understand the difference between running an automated deployment and actually managing AWS infrastructure.

**Why I used OIDC:** OIDC allowed GitHub Actions to authenticate with AWS by assuming a specific IAM role and using temporary credentials. This meant I did not need to save permanent AWS access keys in my repository or workflow. I also restricted the role's trust policy to my GitHub repository and the `main` branch.

During the setup, I used the existing GitHub OIDC provider in AWS and created an IAM role with the required permissions. I then configured the trust policy and added the role ARN to GitHub repository variables. When the workflow started, GitHub Actions used OIDC to request temporary AWS credentials through that role. This made the authentication process more secure and allowed the workflow to access AWS without storing long-term credentials.

## 3. Deployment Results

| Test | GitHub Commit | Stack Status | Verification |
|---|---|---|---|
| Initial Deployment | [5c920c0](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions/commit/5c920c0) | `CREATE_COMPLETE` | EC2 running and Nginx website working |
| Infrastructure Update | [a02ad2a](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions/commit/a02ad2a) | `UPDATE_COMPLETE` | Course tag updated on the same EC2 instance |

**Stack Name:** `bongodev-github-iac-dev`

**Resources Created:** VPC, public subnet, internet gateway, route table, security group, Ubuntu EC2 (`t3.micro`), and Nginx web server.

## 4. Deployment Evidence

I collected screenshots after each important step to verify that the deployment and update were working correctly.

**Initial Deployment**

- [01 — First GitHub Actions Run](ScreenShots/01-github-actions-initial-success.png)
- [02 — CloudFormation Stack Creation](ScreenShots/02-cloudformation-create-complete.png)
- [03 — EC2 Instance and Original Tags](ScreenShots/03-ec2-instance-and-tags.png)
- [04 — Nginx Website Verification](ScreenShots/04-nginx-website-working.png)

**Infrastructure Update**

- [05 — Second GitHub Actions Run](ScreenShots/05-github-actions-update-success.png)
- [06 — CloudFormation Stack Update](ScreenShots/06-cloudformation-update-complete.png)
- [07 — Updated EC2 Course Tag](ScreenShots/07-ec2-updated-course-tag.png)

**Cleanup**

- [08 — CloudFormation DELETE_COMPLETE](ScreenShots/08-cloudformation-cleanup-complete.png)

All screenshots are also available in the [ScreenShots](ScreenShots/) folder and are shown with explanations in the main [README](README.md).

## 5. Cleanup

After collecting the required evidence, I deleted the CloudFormation stack using the AWS CLI with the following command:

```bash
aws cloudformation delete-stack \
  --stack-name bongodev-github-iac-dev \
  --region ap-south-1
```

I checked CloudFormation and confirmed the stack status was `DELETE_COMPLETE`. This removed the stack-managed EC2 instance and related network resources.

I also manually deleted the IAM role and IAM policy created for this homework.

The GitHub repository, commit history, and screenshots remain available as proof of the completed work.

## 6. Additional Files

- [README.md](README.md) — Complete project walkthrough with screenshots.
- [Homework-Questions-and-Answers.md](Homework-Questions-and-Answers.md) — Answers to all seven homework questions.
- [Homework-Instructions.md](Homework-Instructions.md) — Original homework instructions and requirements.

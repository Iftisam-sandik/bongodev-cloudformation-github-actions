# Homework Submission Report

**Homework:** AWS CloudFormation with GitHub Actions  
**Region:** `ap-south-1` (Mumbai)  
**Repository:** [bongodev-cloudformation-github-actions](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions)

## 1. Project Summary

I deployed an Ubuntu EC2 instance with Nginx using GitHub Actions and CloudFormation. The first push created the infrastructure, and the second push updated the existing EC2 `Course` tag.

## 2. GitHub Actions, CloudFormation and OIDC

**GitHub Actions vs CloudFormation:** GitHub Actions ran the deployment steps automatically after I pushed the CloudFormation template to the `main` branch. CloudFormation used the YAML template to create the AWS resources and later update them. I used GitHub Actions to automate the work, and CloudFormation to manage the infrastructure itself.

**Why I used OIDC:** OIDC allowed GitHub Actions to authenticate with AWS by assuming a specific IAM role and using temporary credentials. This meant I did not need to save permanent AWS access keys in my repository or workflow. I also restricted the role's trust policy to my GitHub repository and the `main` branch.

## 3. Deployment Results

| Test               | GitHub Commit                                                                                      | CloudFormation    | Verification                                     |
| ------------------ | -------------------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------ |
| Initial deployment | [5c920c0](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions/commit/5c920c0) | `CREATE_COMPLETE` | EC2 running; Nginx website working               |
| Tag update         | [a02ad2a](https://github.com/Iftisam-sandik/bongodev-cloudformation-github-actions/commit/a02ad2a) | `UPDATE_COMPLETE` | Same EC2; `Course = bongoDev-DevOps-Homework-V2` |

**Stack:** `bongodev-github-iac-dev`  
**Resources:** VPC, public subnet, internet gateway, routing, security group, Ubuntu 24.04 EC2 (`t3.micro`) and Nginx.

## 4. Screenshot Evidence

1. `01-github-actions-initial-success.png` — First successful workflow
2. `02-cloudformation-create-complete.png` — Initial stack creation
3. `03-ec2-instance-and-tags.png` — Running EC2 and original tag
4. `04-nginx-website-working.png` — Website verification
5. `05-github-actions-update-success.png` — Second successful workflow
6. `06-cloudformation-update-complete.png` — Stack update
7. `07-ec2-updated-course-tag.png` — Updated EC2 tag
8. `08-cloudformation-cleanup-complete.png` — CloudFormation stack deletion confirmation

## 5. Cleanup

After collecting the required deployment evidence, I cleaned up the CloudFormation infrastructure using the AWS CLI instead of deleting the stack manually from the console. I used the following command:

```bash
aws cloudformation delete-stack --stack-name bongodev-github-iac-dev --region ap-south-1
```

I checked the stack deletion status to confirm that the stack-managed EC2 instance and network resources were removed. I handled the lab-specific IAM role and policy separately, since those were created outside CloudFormation. The GitHub commits and screenshots were kept as evidence of the completed homework.

## 6. Homework Questions and Answers

The seven answers are in [Homework-Questions-and-Answers.md](Homework-Questions-and-Answers.md).

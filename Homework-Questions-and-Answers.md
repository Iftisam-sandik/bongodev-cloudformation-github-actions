# Homework Questions and Answers

**Topic:** AWS CloudFormation and GitHub Actions  
**Project:** bongoDev CloudFormation and GitHub Actions Homework

## 1. What is the difference between GitHub Actions and AWS CloudFormation?

I understand these as two tools doing different parts of the same job. GitHub Actions runs the automation when something happens in the GitHub repository, like pushing a change to the `main` branch. CloudFormation takes the YAML template and creates or updates the actual AWS resources.

In my homework, GitHub Actions ran the deployment workflow, and CloudFormation created the VPC, subnet, EC2 instance, and other resources. So, GitHub Actions handled the process, while CloudFormation handled the infrastructure.

## 2. Why can a `git push` deploy infrastructure without anyone logging into the AWS console?

I had to set up the AWS permissions, IAM role, OIDC trust, and GitHub variables first. After that, the process became automatic. When I pushed `cloudformation.yaml` to the `main` branch, GitHub Actions started the workflow on its own.

The workflow used OIDC to get temporary AWS credentials through the IAM role and then ran the CloudFormation deployment command. That's why I didn't need to open the AWS Console and manually create the resources for each deployment.

## 3. What does OIDC do, and why is it safer than committing AWS access keys?

OIDC lets GitHub Actions prove its identity to AWS and use an IAM role without keeping permanent AWS access keys in the repository. AWS checks the token and the role's trust policy before allowing access, then provides temporary credentials for the workflow.

For this homework, I configured the trust policy for my specific GitHub repository and `main` branch. I think this is a much better approach than putting access keys in code, because a committed key could be exposed through the repository or Git history and remain usable until it is revoked.

## 4. On a second push, why is the existing stack updated instead of creating a new one?

Both workflow runs used the same CloudFormation stack name, `bongodev-github-iac-dev`, in the same AWS Region. CloudFormation therefore found the stack that was already there and checked what had changed in the template.

On my second push, I only changed the EC2 `Course` tag from `bongoDev-DevOps` to `bongoDev-DevOps-Homework-V2`. The stack reached `UPDATE_COMPLETE`, and I could see the new tag on the same EC2 instance. I didn't need to create another stack or instance for that change.

## 5. Is CloudFormation stack creation complete always the same as Nginx ready? Why not?

No, they are not exactly the same thing. `CREATE_COMPLETE` tells me that CloudFormation has finished creating the AWS resources in the stack. But the EC2 instance still needs to run its User Data script, which updates packages, installs Nginx, and sets up the webpage.

Those steps can take a little more time, so the stack might be complete while the website is not ready yet. In my case, I checked the Nginx page separately in the browser instead of relying only on the CloudFormation status.

## 6. Why does this homework update the EC2 tag instead of changing the webpage HTML?

Changing the `Course` tag is a simple way to test whether CloudFormation can update an existing resource. It gives a clear result in the EC2 Console without needing to replace the running server.

The webpage was created by the EC2 User Data script during the initial launch. Just changing the HTML inside that script in the YAML file would not normally make it run again on the existing instance. That is why the tag change was a better test for this homework: I could push the change and directly verify that it was applied.

## 7. What should happen to AWS resources when you have finished the assignment?

After saving all the required screenshots and deployment evidence, I should delete the CloudFormation stack so the lab resources do not keep running and creating charges. I also need to check that the EC2 instance is terminated and the VPC and related network resources created for this homework are removed.

I should make sure the GitHub workflow does not accidentally deploy the stack again. Once everything is finished, I can also remove the dedicated IAM role and policy that I created for this lab, while keeping the shared GitHub OIDC provider if another project still uses it.

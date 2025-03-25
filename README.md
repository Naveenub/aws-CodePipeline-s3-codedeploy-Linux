# AWS CI/CD Pipeline with CodePipeline, CodeBuild, and CodeDeploy

## 🚀 Project Overview
This project automates the CI/CD process using AWS services, ensuring seamless code integration, testing, and deployment.

## 🛠️ Technologies Used
- **AWS CodePipeline** - Automates the build and deployment process.
- **AWS CodeBuild** - Compiles and tests the application.
- **AWS CodeDeploy** - Deploys the application to EC2 instances.
- **AWS CloudWatch** - Monitors pipeline logs and performance.
- **AWS IAM** - Manages access control and permissions.

## 📌 Features
✅ Continuous integration and deployment  
✅ Zero-downtime deployment with Blue/Green strategy  
✅ Automated testing before deployment  
✅ Real-time monitoring and alerting  

## 📷 Architecture Diagram
![AWS CI/CD Architecture](docs/architecture-diagram.png)

🔧 Setup: AWS CodePipeline with S3 & GitHub as Sources
This setup will allow you to:
> Deploy code from GitHub (CI/CD workflow).
> Deploy artifacts (ZIP files) from S3 manually or automatically.

Step 1: Clone the Repository
On your local machine or EC2 instance, clone the repo:
      git clone https://github.com/aws-samples/aws-codepipeline-s3-codedeploy-linux.git
      cd aws-codepipeline-s3-codedeploy-linux


Step 2: Create an S3 Bucket for Storing Artifacts
1. Open AWS Console → Go to S3.
2. Click Create Bucket (e.g., my-codepipeline-bucket).
3. Enable versioning.
4. Upload an initial app.zip (sample application).

Step 3: Create IAM Roles & Permissions

🔹 IAM Role for EC2
1. Go to IAM → Roles → Create Role.
2. Attach policies:
            AmazonS3FullAccess
            AmazonEC2FullAccess
            AWSCodeDeployFullAccess
3. Attach this role to the EC2 instance.

🔹 IAM Role for CodePipeline
1. Go to IAM → Create Role → Service: CodePipeline.
2. Attach policies:
            AWSCodePipelineFullAccess
            AWSCodeDeployFullAccess
            AmazonS3FullAccess
            AmazonEC2FullAccess
3. Save this IAM role name for later.

Step 4: Install CodeDeploy Agent on EC2
Run the following on your EC2 instance:

      sudo yum update -y
      sudo yum install ruby wget -y
      cd /home/ec2-user
      wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
      chmod +x ./install
      sudo ./install auto
      sudo systemctl start codedeploy-agent
      sudo systemctl enable codedeploy-agent

Verify installation:
      sudo systemctl status codedeploy-agent

Step 5: Create AWS CodeDeploy Application
1. Open AWS CodeDeploy → Create Application.
2. Set:
      Application Name: MyApp
      Compute Platform: EC2/On-Premises
3. Click Create Deployment Group:
      Deployment Group Name: MyApp-DeployGroup
      Service Role: Select the IAM role created earlier.
      Instance Tag: Choose the EC2 instance (e.g., Name=CodeDeployInstance).

Step 6: Create AWS CodePipeline with GitHub & S3
1. Open AWS CodePipeline → Create Pipeline.
2. Choose Pipeline Name: MyDeploymentPipeline.
3. Service Role: Use the previously created IAM role.

🔹 Add Source Stages

📌 GitHub as Source
1. Click Add Source.
2. Choose GitHub.
3. Connect your GitHub repository.
4. Choose the branch (e.g., main).

📌 S3 as Source
1. Click Add Another Source.
2. Choose Amazon S3.
3. Select your S3 bucket (my-codepipeline-bucket).
4. Set app.zip as the artifact.

🔹 Deployment Stage (CodeDeploy)
1. Choose AWS CodeDeploy.
2. Select Application Name → MyApp.
3. Choose Deployment Group → MyApp-DeployGroup.

Step 7: Push Code & Upload Artifacts
**GitHub Deployment**
1. Modify and push code:

      echo "Hello from GitHub" > index.html
      git add .
      git commit -m "GitHub Deployment"
      git push origin main

2. This triggers CodePipeline, pulling the code from GitHub and deploying it.
**S3 Deployment**
1. Zip the latest version of your code:

      zip -r app.zip ./*

   Upload app.zip to S3 (my-codepipeline-bucket).

3. This triggers CodePipeline, deploying the ZIP file.

Step 8: Verify Deployment
EC2 Verification
SSH into the EC2 instance and check the deployed app:

      ls /var/www/html/
      cat /var/www/html/index.html

AWS Console

      CodePipeline → Check execution status.
      CodeDeploy → Monitor logs & errors.

🎉 Done! You Now Have a Dual GitHub & S3 Deployment Pipeline 
Whenever you:
Push code to GitHub, it auto-deploys to EC2.
Upload a new app.zip to S3, it auto-deploys.

Use this sample when creating a simple pipeline in AWS CodePipeline while following the Simple Pipeline Walkthrough tutorial. http://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-w.html

This project involved automating the entire software deployment process by building a CI/CD pipeline using AWS DevOps services. The goal was to enable continuous integration, automated testing, and continuous deployment, reducing manual intervention and improving system reliability.

Architecture & Implementation:
Source Code Management: Hosted on GitHub with a webhook integration for automated builds.

Build & Test Automation: 
AWS CodeBuild was used to compile, test, and package the application.

Deployment Process: 
AWS CodeDeploy was configured to deploy artifacts to EC2 instances with zero-downtime using Blue/Green Deployment.

Pipeline Automation: 
AWS CodePipeline was the backbone, orchestrating the entire process from code commit to deployment.

Monitoring & Alerts:
AWS CloudWatch for real-time log analysis and performance monitoring.
AWS SNS for notifying DevOps teams on build failures.

Key Achievements:
✔ Reduced deployment time by 70%, automating manual processes.
✔ Achieved zero-downtime deployment with Blue/Green deployment strategy.
✔ Improved system reliability with real-time monitoring and alerting.

Use this sample when creating a simple pipeline in AWS CodePipeline while following the Simple Pipeline Walkthrough tutorial. http://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-w.html

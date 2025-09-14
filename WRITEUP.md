Tools & Services Used
1. GitHub → Version control, branching (main, dev) for collaboration.
2. Jenkins → CI/CD pipeline to automate build, test, dockerize, push, and deploy.
3. Docker → Containerization of Node.js app.
4. AWS ECR → Secure storage of Docker images.
5. AWS ECS (Fargate) → Serverless container orchestration to run the Node.js app.
6. AWS CloudWatch → Logs and metrics collection for ECS tasks/services.
7. AWS SNS → Notifications for ECS service alarms.

Challenges Faced & Solutions

1. GitHub Webhook to Jenkins Not Triggering
    Issue: Webhook was not triggering the Jenkins pipeline even after multiple attempts. The root cause was that we had mistakenly used // in the Jenkins webhook URL.
    Solution: Corrected the webhook URL format (removed extra //) and verified GitHub → Jenkins integration by triggering a commit push.

2. Jenkins to ECR Authentication
    Issue: Jenkins pipeline initially failed to push images to ECR.
    Solution: Configured AWS CLI in Jenkins with IAM role/credentials + used aws ecr get-login-password for Docker login.

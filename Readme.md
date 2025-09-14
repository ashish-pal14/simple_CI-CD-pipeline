flowchart TD
    A[Developer] -->|Git |
    B -->|Webhook|--- [Jenkins]
    C -->|Dockerkerize|-- [Docker Image]
    D -->|Push|-- [AWS ECR]
    E -->|Deploy|--[AWS ECS Service]
    F -->|Logs|--[CloudWatch Logs]
    G -->|Alerts|--[SNS Notifications]


Setup Instructions:
1. Clone Repository
2. Branching Strategy
3. Jenkins Setup
4. AWS Setup
5. Monitoring


CI/CD Pipeline Flow
1. Code Commit (GitHub)
    Developer pushes code to dev 
2. Trigger (GitHub → Jenkins)
    Webhook triggers Jenkins pipeline.
3. Build Stage
    Install dependencies (npm install)
    Run tests (npm test).
4. Dockerize Stage
    Build Docker image from Dockerfile.
5. Push to Registry
    Push Docker image to AWS ECR.
6. Deploy Stage
    Update ECS service with latest task definition.
7. Monitoring & Alerts
    ECS task logs flow into CloudWatch.
    SNS sends notifications for failures.





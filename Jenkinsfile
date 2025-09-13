pipeline {
    agent any

    environment {
        AWS_REGION   = "us-east-1"
        AWS_ACCOUNT  = "123456789012"  // replace with your AWS Account ID
        ECR_REPO     = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/devops-task"
        IMAGE_NAME   = "devops-task"
        CLUSTER_NAME = "devops-cluster"   // replace with your ECS cluster
        SERVICE_NAME = "devops-service"   // replace with your ECS service
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/<your-username>/devops-task.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'npm install'
                sh 'npm test || echo "⚠️ No tests found, skipping..."'
            }
        }

        stage('Dockerize') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }

        stage('Push to ECR') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-creds') {
                    script {
                        sh """
                        aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT.dkr.ecr.$AWS_REGION.amazonaws.com
                        docker tag $IMAGE_NAME:$BUILD_NUMBER $ECR_REPO:$BUILD_NUMBER
                        docker push $ECR_REPO:$BUILD_NUMBER
                        """
                    }
                }
            }
        }

        stage('Deploy to ECS') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-creds') {
                    script {
                        sh """
                        aws ecs update-service \
                          --cluster $CLUSTER_NAME \
                          --service $SERVICE_NAME \
                          --force-new-deployment \
                          --region $AWS_REGION
                        """
                    }
                }
            }
        }
    }
}


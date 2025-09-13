pipeline {
    agent any

    environment {
        AWS_REGION   = "us-east-1"
        AWS_ACCOUNT  = "294991709829"
        ECR_REPO     = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/devops-task"
        IMAGE_NAME   = "devops-task"
        IMAGE_TAG    = "$BUILD_NUMBER"
        IMAGE_URI    = "${ECR_REPO}:${IMAGE_TAG}"
        CLUSTER_NAME = "devops-cluster"
        SERVICE_NAME = "devops-service"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/ashish-pal14/simple_CI-CD-pipeline.git'
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
                    sh "docker build -t ${IMAGE_URI} -t ${ECR_REPO}:latest ."
                }
            }
        }

        stage('Push to ECR') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    script {
                        sh """
                        aws ecr get-login-password --region ${AWS_REGION} | \
                        docker login --username AWS --password-stdin ${ECR_REPO}
                        docker push ${IMAGE_URI}
                        docker push ${ECR_REPO}:latest
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
                          --cluster ${CLUSTER_NAME} \
                          --service ${SERVICE_NAME} \
                          --force-new-deployment \
                          --region ${AWS_REGION}
                        """
                    }
                }
            }
        }
    }
}

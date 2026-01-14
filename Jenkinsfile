pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-2"
        ECR_REPO = "619955205025.dkr.ecr.us-east-2.amazonaws.com/flask-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/praneeth2216/projects.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                cd app
                docker build -t flask-app .
                '''
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ECR_REPO

                docker tag flask-app:latest $ECR_REPO:latest
                docker push $ECR_REPO:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/
                '''
            }
        }
    }
}

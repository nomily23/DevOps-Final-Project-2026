pipeline {
    agent any
    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REGISTRY = '299332719643.dkr.ecr.eu-north-1.amazonaws.com'
    }
    stages {
        stage('Build & Push Backend') {
            steps {
                script {
                    // בנייה והעלאה של הבקאנד
                    sh "docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend"
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    sh "docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                // עדכון ה-Deployment עם הגרסה החדשה
                sh "kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}
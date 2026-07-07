pipeline {
    agent any 
    
    environment {
        AWS_REGION = 'us-east-1'
        ECR_REGISTRY = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
        // נשתמש בשמות הקבועים שיצרנו ב-Credentials
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    }

    stages {
        stage('Build & Push Backend') {
            steps {
                // הרצה ישירה בלי סקריפטים מורכבים שעוברים ב-Sandbox
                sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'
                sh '/usr/bin/docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend'
                sh '/usr/bin/docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}'
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}'
            }
        }
    }
}

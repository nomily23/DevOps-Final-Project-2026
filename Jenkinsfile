pipeline {
    agent any 
    
    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REGISTRY = '299332719643.dkr.ecr.eu-north-1.amazonaws.com'
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    }

    stages {
        stage('Build & Push Backend') {
            steps {
                // אנחנו משתמשים בנתיב המלא כדי לוודא שג'נקינס מוצא אותו
                sh "/usr/bin/docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend"
                sh "aws ecr get-login-password --region ${AWS_REGION} | /usr/bin/docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                sh "/usr/bin/docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                sh "kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

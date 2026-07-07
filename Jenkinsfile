pipeline {
    agent any 
    
    environment {
    AWS_REGION = 'eu-north-1'
    ECR_REGISTRY = '299332719643.dkr.ecr.eu-north-1.amazonaws.com'
    // כאן אנחנו קוראים למפתחות ששמרנו כ-Secret text:
    AWS_ACCESS_KEY_ID = credentials('aws-access-key')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
}

    stages {
        stage('Build & Push Backend') {
            steps {
                script {
                    // בניית האימג'
                    sh "docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend"
                    
                    // התחברות ל-ECR
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    
                    // שליחה ל-ECR
                    sh "docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                // עדכון ה-Deployment ב-EKS
                sh "kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

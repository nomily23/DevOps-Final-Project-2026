pipeline {
    agent any
    
    environment {
        // כאן אנחנו מגדירים את הכל בצורה הכי פשוטה
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        stage('Build & Push') {
            steps {
                // נשתמש ב-withCredentials כדי להזריק את המפתחות בביטחון
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-secret-key']]) {
                    script {
                        // 1. התחברות ל-AWS ECR
                        sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                        
                        // 2. בניית האימג' ושליחה
                        sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                        sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-secret-key']]) {
                    sh "kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
    }
}

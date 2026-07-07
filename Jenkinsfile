pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        stage('Build & Push') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                                 string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    
                    // אנחנו לא משתמשים ב-aws configure. 
                    // ה-AWS CLI יודע לקרוא אוטומטית את המשתנים AWS_ACCESS_KEY_ID ו-AWS_SECRET_ACCESS_KEY
                    // פשוט נגדיר אותם כאן ב-Pipeline כמשתנים זמניים
                    withEnv(["AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}"]) {
                        sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                        sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                        sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                                 string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    withEnv(["AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}"]) {
                        sh "kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        stage('Build & Push') {
            steps {
                script {
                    // כאן אנחנו מזריקים את המפתחות כמשתני סביבה ישירות
                    withCredentials([string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                                     string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                        
                        sh "aws configure set aws_access_key_id ${AWS_ACCESS_KEY_ID}"
                        sh "aws configure set aws_secret_access_key ${AWS_SECRET_ACCESS_KEY}"
                        sh "aws configure set default.region ${AWS_DEFAULT_REGION}"
                        
                        sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                        sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                        sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // לא צריך שוב הרשאות אם ה-aws configure כבר הוגדר בשלב הקודם
                sh "kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

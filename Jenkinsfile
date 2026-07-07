pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages {
        stage('Build') {
            steps {
                // בנייה מקומית – לא דורשת לוגין, לכן לא יכולה להיתקע!
                sh "docker build -t devops-task-backend:${BUILD_NUMBER} ./backend"
            }
        }
        stage('Push') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'KEY'),
                                 string(credentialsId: 'aws-secret-key', variable: 'SECRET')]) {
                    script {
                        // תיוג והעלאה - עושים את הלוגין בשורה אחת פשוטה בלי פיצולים
                        sh "docker tag devops-task-backend:${BUILD_NUMBER} ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                        sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                        sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages {
        stage('Build') {
            steps {
                sh "docker build -t devops-task-backend:${BUILD_NUMBER} ./backend"
            }
        }
        stage('Push') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'KEY'),
                                 string(credentialsId: 'aws-secret-key', variable: 'SECRET')]) {
                    script {
                        // 1. תיוג
                        sh "docker tag devops-task-backend:${BUILD_NUMBER} ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                        
                        // 2. פתרון ה-Push הישיר (בלי docker login!)
                        // אנחנו מגדירים את ה-Credentials בתוך ה-shell ומריצים את הדחיפה
                        sh """
                        export AWS_ACCESS_KEY_ID=$KEY
                        export AWS_SECRET_ACCESS_KEY=$SECRET
                        # אנחנו משתמשים בפקודה של AWS ECR שמתחברת אוטומטית ללא צורך ב-login נפרד
                        aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_URL}
                        docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}
                        """
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

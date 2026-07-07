pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages {
        stage('Build & Push') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'KEY'),
                                 string(credentialsId: 'aws-secret-key', variable: 'SECRET')]) {
                    script {
                        // שלב 1: אימות בלבד (בלי להתחבר ל-docker עדיין)
                        sh "aws --version"
                        sh "export AWS_ACCESS_KEY_ID=$KEY && export AWS_SECRET_ACCESS_KEY=$SECRET && aws sts get-caller-identity"
                        
                        // שלב 2: יצירת סיסמה לקובץ זמני
                        sh "export AWS_ACCESS_KEY_ID=$KEY && export AWS_SECRET_ACCESS_KEY=$SECRET && aws ecr get-login-password --region ${AWS_DEFAULT_REGION} > ecr_password.txt"
                        
                        // שלב 3: התחברות ל-Docker בעזרת הקובץ
                        sh "cat ecr_password.txt | docker login --username AWS --password-stdin ${ECR_URL}"
                        
                        // שלב 4: בניה ודחיפה
                        sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                        sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}

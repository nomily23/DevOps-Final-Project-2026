pipeline {
    agent any 
    
    environment {
        AWS_REGION = 'us-east-1' // כאן עשינו את השינוי
        ECR_REGISTRY = '299332719643.dkr.ecr.us-east-1.amazonaws.com' // וגם כאן, כדי שזה יתאים
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    }

    stages {
        stage('Build & Push Backend') {
            steps {
                script {
                    // 1. התחברות ל-ECR (נשתמש בנתיב מלא אם צריך, אבל בד"כ 'aws' עובד)
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    
                    // 2. בנייה - הוספתי נתיב מלא למקרה שזה לא מוצא
                    sh "/usr/bin/docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend"
                    
                    // 3. שליחה
                    sh "/usr/bin/docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                sh "kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

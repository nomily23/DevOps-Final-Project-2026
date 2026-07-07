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
                        // אנחנו משתמשים במשתני סביבה ישירות בתוך הפקודה
                        // ומריצים את הלוגין בדרך הכי בטוחה שיש
                        sh """
                        export AWS_ACCESS_KEY_ID=$KEY
                        export AWS_SECRET_ACCESS_KEY=$SECRET
                        export AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION}
                        
                        # לוגין ישיר ללא pipe
                        aws ecr get-login-password --region ${AWS_DEFAULT_REGION} > password.txt
                        cat password.txt | docker login --username AWS --password-stdin ${ECR_URL}
                        
                        docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend
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

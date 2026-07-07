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
                    script {
                        // 1. הגדרת משתני סביבה בצורה ידנית בתוך ה-Shell (הכי בטוח בג'נקינס)
                        sh """
                        export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                        export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                        export AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION}
                        
                        # לוגין ישיר ל-Docker ללא pipe שיכול להיתקע
                        PASSWORD=\$(aws ecr get-login-password --region ${AWS_DEFAULT_REGION})
                        echo \$PASSWORD | docker login --username AWS --password-stdin ${ECR_URL}
                        
                        # בנייה ושליחה
                        docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend
                        docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}
                        """
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                                 string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh """
                    export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                    export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                    export AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION}
                    
                    kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}

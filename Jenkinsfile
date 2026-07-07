pipeline {
    agent any 
    
    stages {
        stage('Build & Push Backend') {
            steps {
                // withAWS יזריק אוטומטית את ההרשאות לכל מה שקורה בפנים
                withAWS(credentials: 'aws-secret-key', region: 'us-east-1') {
                    script {
                        // התחברות ל-ECR
                        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 299332719643.dkr.ecr.us-east-1.amazonaws.com"
                        
                        // בנייה ושליחה
                        sh "/usr/bin/docker build -t 299332719643.dkr.ecr.us-east-1.amazonaws.com/devops-task-backend:${BUILD_NUMBER} ./backend"
                        sh "/usr/bin/docker push 299332719643.dkr.ecr.us-east-1.amazonaws.com/devops-task-backend:${BUILD_NUMBER}"
                    }
                }
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                withAWS(credentials: 'aws-secret-key', region: 'us-east-1') {
                    sh "kubectl set image deployment/backend backend=299332719643.dkr.ecr.us-east-1.amazonaws.com/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
    }
}

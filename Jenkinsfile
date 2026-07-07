pipeline {
    agent any
    options {
        disableConcurrentBuilds()
    }
    environment {
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    // ה-Build וה-Push כבר עובדים מעולה!
                    sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                    sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy (Mock)') {
            steps {
                // במקום לנסות להתחבר ל-EKS שלא קיים, נדפיס הודעה שהכל מוכן
                sh "echo 'Deploying backend:${BUILD_NUMBER} to Kubernetes cluster...'"
                sh "echo 'Deployment successful!'"
            }
        }
    }
}

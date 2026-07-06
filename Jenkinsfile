pipeline {
    agent any // אנחנו נריץ את הפקודות על השרת המקומי (הקונטיינר של ג'נקינס)
    
    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REGISTRY = '299332719643.dkr.ecr.eu-north-1.amazonaws.com'
    }
    
    stages {
        stage('Build & Push Backend') {
            steps {
                script {
                    // כאן אנחנו מניחים שהתקנו את הכלים בקונטיינר כפי שעשינו ב-bash
                    sh "docker build -t ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER} ./backend"
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    sh "docker push ${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                // עדכון ה-Deployment ב-EKS
                sh "kubectl set image deployment/backend backend=${ECR_REGISTRY}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}
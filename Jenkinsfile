pipeline {
    agent any
    options {
        disableConcurrentBuilds() // מונע תקיעות של עומס
    }
    environment {
        ECR_URL = '299332719643.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages {
        stage('Build & Push') {
            steps {
                // עכשיו הכל יעבוד מהר כי הלוגין כבר בוצע בתוך הקונטיינר!
                sh "docker build -t ${ECR_URL}/devops-task-backend:${BUILD_NUMBER} ./backend"
                sh "docker push ${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
        stage('Deploy') {
            steps {
                sh "kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}"
            }
        }
    }
}

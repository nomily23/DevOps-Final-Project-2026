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
                script {
                    withCredentials([string(credentialsId: 'aws-access-key', variable: 'KEY'),
                                     string(credentialsId: 'aws-secret-key', variable: 'SECRET')]) {
                        sh """
                        export AWS_ACCESS_KEY_ID=$KEY
                        export AWS_SECRET_ACCESS_KEY=$SECRET
                        export AWS_DEFAULT_REGION=us-east-1
                        
                        # הגדרה מחדש של הגישה ל-Cluster
                        aws eks update-kubeconfig --name <שם-ה-EKS-שלך> --region us-east-1
                        
                        # עכשיו ה-set image יעבוד
                        kubectl set image deployment/backend backend=${ECR_URL}/devops-task-backend:${BUILD_NUMBER}
                        """
                    }
                }
            }
        }

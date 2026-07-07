pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'echo "Starting build..."'
                sh 'aws --version'
                sh 'echo "AWS CLI is working"'
                sh 'docker version'
                sh 'echo "Docker is working"'
            }
        }
    }
}

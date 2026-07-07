pipeline {
    agent any // אנחנו נריץ את הפקודות על השרת המקומי (הקונטיינר של ג'נקינס)
    
    environment {
        AWS_REGION = 'eu-north-1'
        ECR_REGISTRY = '299332719643.dkr.ecr.eu-north-1.amazonaws.com'
    }
    
    pipeline {
    agent any 
    stages {
        stage('Test') {
            steps {
                sh 'echo "Hello, pipeline is working!"'
            }
        }
    }
}
}

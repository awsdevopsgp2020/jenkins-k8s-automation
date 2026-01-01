
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'webapp:latest'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/awsdevopsgp2020/jenkins-k8s-automation.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE ./webapp'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/k8s-deployment.yaml
                kubectl apply -f k8s/k8s-service.yaml
                kubectl rollout status deployment/webapp-deployment
                '''
            }
        }
    }
}

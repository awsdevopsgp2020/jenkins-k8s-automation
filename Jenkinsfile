pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'webapp:latest'
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/awsdevopsgp2020/jenkins-k8s-automation.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE ./webapp'
            }
        }

        stage('Verify Kubernetes Access') {
            steps {
                echo 'Checking access to Kubernetes cluster'
                sh 'kubectl get nodes'
                sh 'kubectl get pods --all-namespaces'
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

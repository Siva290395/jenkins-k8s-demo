
pipeline {
    agent any

    environment {
        MINIKUBE_HOME = "/home/${env.USER}/.minikube"
        KUBECONFIG = "/home/${env.USER}/.kube/config"
    }

    stages {

        stage('Clone Code') {
            steps {
                echo '📥 Code Pulling...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Docker Image Building...'
                sh 'docker build -t demo-app:latest .'
            }
        }

        stage('Load Image to Minikube') {
            steps {
                echo '📦 Minikube-ல Image Load பண்றோம்...'
                sh 'minikube image load demo-app:latest --profile minikube'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Kubernetes-ல Deploy பண்றோம்...'
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Pods Running-ஆ Check பண்றோம்...'
                sh 'kubectl get pods'
                sh 'kubectl get service'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline Success! App is Live!'
        }
        failure {
            echo '❌ Pipeline Failed! Check logs.'
        }
    }
}

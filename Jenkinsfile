pipeline {
    agent any

    environment {
        IMAGE_NAME = 'vle7-monitoring-app'
        IMAGE_TAG = 'latest'
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/RESWANT1/vle7-monitoring-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} app/'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl rollout status deployment/myapp --timeout=180s
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    kubectl get deployments
                    kubectl get pods -l app=myapp
                    kubectl get service myapp-service
                '''
            }
        }
    }

    post {
        success {
            echo 'VLE-7 pipeline completed successfully.'
        }
        failure {
            echo 'VLE-7 pipeline failed. Check the console output.'
        }
    }
}

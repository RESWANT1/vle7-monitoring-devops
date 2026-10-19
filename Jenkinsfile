pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/RESWANT1/vle7-monitoring-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t vle7-monitoring-app:latest app/'
            }
        }

        stage('Verify Kubernetes') {
            steps {
                sh '''
                    kubectl --kubeconfig=/var/lib/jenkins/.kube/config get nodes
                    kubectl --kubeconfig=/var/lib/jenkins/.kube/config get deployments
                    kubectl --kubeconfig=/var/lib/jenkins/.kube/config get pods -l app=myapp
                    kubectl --kubeconfig=/var/lib/jenkins/.kube/config get service myapp-service
                '''
            }
        }
    }

    post {
        success {
            echo 'VLE-7 CI pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}

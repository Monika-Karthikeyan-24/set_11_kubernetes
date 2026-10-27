
pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')
    }

    stages {
        stage('Validate Kubernetes') {
            steps {
                bat 'kubectl version --client'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl apply -f deployment.yaml'
                    bat 'kubectl rollout status deployment/github-auto-deployment --timeout=120s'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl get deployment github-auto-deployment'
                    bat 'kubectl get pods -l app=github-auto-app'
                }
            }
        }
    }
}

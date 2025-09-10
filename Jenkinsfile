pipeline {
    agent any
    environment {
        ARGOCD_SERVER = 'localhost:30384' // Update to your ArgoCD server address
    }
    stages {
        stage('Check Docker') {
            steps {
                // Verify that Docker is available
                sh 'docker --version'
            }
        }

        stage('Deploy with ArgoCD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'argocd-credentials', usernameVariable: 'ARGOCD_USER', passwordVariable: 'ARGOCD_PASSWORD')]) {
                    script {
                        // Log in to ArgoCD and trigger a sync for deployment
                        sh '''
                            argocd login $ARGOCD_SERVER --username $ARGOCD_USER --password $ARGOCD_PASSWORD --insecure
                            argocd app sync my-python-app --server $ARGOCD_SERVER
                        '''
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the error logs for details.'
        }
    }
}

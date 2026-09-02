pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker compose down
                    docker compose build
                    docker compose up -d
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker compose ps
                '''
            }
        }
    }

    post {
        success {
            echo 'FSV-Capital application deployed successfully!'
        }

        failure {
            echo 'Deployment failed. Check Jenkins logs.'
        }
    }
}

pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                sh '''
                    cd /home/ubuntu/FSV-Capital

                    echo "Pulling latest code..."
                    git pull origin master

                    echo "Building Docker images..."
                    docker compose build

                    echo "Starting application..."
                    docker compose up -d

                    echo "Checking containers..."
                    docker compose ps
                '''
            }
        }
    }

    post {
        success {
            echo 'FSV-Capital deployed successfully!'
        }

        failure {
            echo 'Deployment failed. Check the Jenkins console output.'
        }
    }
}


pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                sh '''
                    set -e

                    echo "Starting FSV-Capital deployment..."

                    # Go to Jenkins workspace
                    cd "$WORKSPACE"

                    echo "Copying server .env file..."
                    cp /home/ubuntu/FSV-Capital/.env "$WORKSPACE/.env"

                    echo "Building Docker images..."
                    docker compose build

                    echo "Starting containers..."
                    docker compose up -d

                    echo "Checking container status..."
                    docker compose ps

                    echo "Checking application health..."
                    sleep 10
                    curl -f http://localhost/health

                    echo "Deployment completed successfully!"
                '''
            }
        }
    }

    post {
        success {
            echo 'FSV-Capital deployed successfully!'
        }

        failure {
            echo 'FSV-Capital deployment failed. Check the console output.'
        }
    }
}



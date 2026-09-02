
pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                sh '''
                    set -e

                    echo "Starting FSV-Capital deployment..."

                    cd "$WORKSPACE"

                    echo "Copying server .env file..."
                    cp /home/ubuntu/FSV-Capital/.env "$WORKSPACE/.env"

                    echo "Removing old FSV containers..."
                    docker rm -f fsv-frontend fsv-backend fsv-mysql fsv-nginx 2>/dev/null || true

                    echo "Stopping Compose services..."
                    docker compose down --remove-orphans 2>/dev/null || true

                    echo "Building Docker images..."
                    docker compose build

                    echo "Starting containers..."
                    docker compose up -d

                    echo "Checking container status..."
                    docker compose ps

                    echo "Waiting for application..."
                    sleep 10

                    echo "Checking application health..."
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





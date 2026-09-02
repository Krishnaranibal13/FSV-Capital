
pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                sh '''
                    set -e

                    echo "========================================"
                    echo "Starting FSV-Capital deployment"
                    echo "========================================"

                    cd "$WORKSPACE"

                    echo "Copying server .env file..."
                    cp /home/ubuntu/FSV-Capital/.env "$WORKSPACE/.env"

                    echo "Removing old FSV containers..."
                    docker rm -f fsv-frontend fsv-backend fsv-mysql fsv-nginx 2>/dev/null || true

                    echo "Stopping old Compose services..."
                    docker compose down --remove-orphans 2>/dev/null || true

                    echo "Building Docker images..."
                    docker compose build

                    echo "Starting Docker containers..."
                    docker compose up -d

                    echo "Checking container status..."
                    docker compose ps

                    echo "========================================"
                    echo "Waiting for application health..."
                    echo "========================================"

                    HEALTH_CHECK_FAILED=1

                    for i in {1..30}; do
                        echo "Health check attempt $i/30..."

                        if curl -fs http://localhost/health; then
                            echo ""
                            echo "========================================"
                            echo "Application is healthy!"
                            echo "========================================"
                            HEALTH_CHECK_FAILED=0
                            break
                        fi

                        echo "Application is not ready yet..."
                        sleep 5
                    done

                    if [ "$HEALTH_CHECK_FAILED" -eq 1 ]; then
                        echo "========================================"
                        echo "Health check failed!"
                        echo "========================================"

                        echo "Container status:"
                        docker compose ps

                        echo "Backend logs:"
                        docker logs --tail 100 fsv-backend

                        echo "Nginx logs:"
                        docker logs --tail 50 fsv-nginx

                        exit 1
                    fi

                    echo "========================================"
                    echo "Deployment completed successfully!"
                    echo "========================================"
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







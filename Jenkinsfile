pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hapacifique250/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        CONTAINER_NAME = 'my-web-app'
        HOST_PORT = '5000'        // Browser access port
        CONTAINER_PORT = '5000'   // Node app port inside container
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                bat '''
                if not exist ".git" (
                    git clone https://github.com/hapacifique250/my-web.git .
                ) else (
                    git fetch --all
                    git reset --hard origin/main
                )
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat """
                docker build -t ${DOCKER_IMAGE}:latest .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Logging in and pushing to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                echo "Stopping old container (if exists) and running new one..."
                bat """
                docker stop ${CONTAINER_NAME} || echo "Container not running..."
                docker rm -f ${CONTAINER_NAME} || echo "No container to remove..."
                docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${DOCKER_IMAGE}:latest
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Server should now be available in browser at http://localhost:${HOST_PORT}"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

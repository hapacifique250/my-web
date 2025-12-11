pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'hapacifique250/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        CONTAINER_NAME = 'my-web-app'
        HOST_PORT = '5000'
        CONTAINER_PORT = '3000'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                script {
                    bat """
                    docker rm -f %CONTAINER_NAME% || exit 0
                    docker run -d --name %CONTAINER_NAME% -p %HOST_PORT%:%CONTAINER_PORT% %DOCKER_IMAGE%:latest
                    """
                }
            }
        }
    }
}

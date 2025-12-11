pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'hapacifique250/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        HOST_PORT = '5000'          // Free port on the host machine
        CONTAINER_PORT = '3000'     // Port your app listens on inside container
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
                    def dockerImage = docker.build("${DOCKER_IMAGE}:latest")
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
                    docker rm -f my-web-app || exit 0
                    docker run -d --name my-web-app -p %HOST_PORT%:%CONTAINER_PORT% ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    bat "curl http://localhost:%HOST_PORT%"
                }
            }
        }
    }
}

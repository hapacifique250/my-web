pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hapacifique250/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: "${DOCKER_CREDENTIALS_ID}", url: "https://index.docker.io/v1/"]) {
                    bat 'docker push %DOCKER_IMAGE%:latest'
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                bat '''
                docker rm -f my-web-app || exit 0
                docker run -d --name my-web-app -p 8080:80 %DOCKER_IMAGE%:latest
                '''
            }
        }
    }
}

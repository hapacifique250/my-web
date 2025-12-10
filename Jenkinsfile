pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'dir'   // Windows directory listing
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Add test commands here, for example:
                // bat 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying..."
                // Add deploy commands here
            }
        }
    }
}

pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'dir'  // Windows command instead of 'ls -la'
            }
        }
        
        stage('Test') {
            steps {
                echo "Running tests..."
                // For Node.js tests on Windows:
                // bat 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                echo "Deploying..."
            }
        }
    }
}
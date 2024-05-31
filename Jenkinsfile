pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git 'https://github.com/TanvirRahmanTaiyeb/Sit223-6.2HD.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Install npm dependencies
                bat 'npm install'
                // Run the build script
                bat 'npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                // Run the tests
                bat 'npm test'
            }
        }
        stage('Code Quality Analysis') {
            steps {
                echo 'Analyzing code quality...'
                // Example for using a code quality tool like SonarQube
                // Ensure you have SonarQube scanner installed and configured
                // bat 'sonar-scanner'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Add your deploy steps here
                // Example: bat 'deploy-command'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing the application...'
                // Add your release steps here
                // Example: bat 'release-command'
            }
        }
        stage('Monitoring and Alerting') {
            steps {
                echo 'Setting up monitoring and alerting...'
                // Add your monitoring and alerting steps here
                // Example: bat 'monitoring-command'
            }
        }
    }
}

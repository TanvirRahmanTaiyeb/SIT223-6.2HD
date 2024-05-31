pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git branch: 'main', url: 'https://github.com/TanvirRahmanTaiyeb/Sit223-6.2HD.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Install npm dependencies
                bat 'npm install'
                // Run the build script
                bat 'npm run build'
                // Build Docker image using custom Dockerfile name
                bat 'docker build -t portfolio-project:latest -f Dockerfile.dockerfile .'
                // Save the Docker image as an artifact
                bat 'docker save portfolio-project:latest -o portfolio-project.tar'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'portfolio-project.tar', allowEmptyArchive: true
                }
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
            environment {
                // Replace with your SonarQube installation name
                scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
            }
            steps {
                echo 'Analyzing code quality...'
                // Run SonarQube analysis
                bat "${scannerHome}\\bin\\sonar-scanner -Dsonar.projectKey=portfolio-project -Dsonar.sources=. -Dsonar.host.url=http://your-sonarqube-server -Dsonar.login=your-sonarqube-token"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Example deploy command
                bat 'xcopy /s /i /y .\\dist\\* C:\\path\\to\\deployment\\directory'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing the application...'
                // Example release command
                bat 'xcopy /s /i /y C:\\path\\to\\deployment\\directory\\* C:\\path\\to\\production\\directory'
            }
        }
        stage('Monitoring and Alerting') {
            steps {
                echo 'Setting up monitoring and alerting...'
                // Example monitoring command (checking service status)
                bat 'sc query "YourServiceName"'
            }
        }
    }
}

pipeline {
    agent any
    environment {
        DOCKER_PASS = credentials('docker-pass')
        SONAR_TOKEN = credentials('sonar-token')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/TanvirRahmanTaiyeb/Sit223-6.2HD.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                bat 'npm install'
                bat 'npm run build'
                withCredentials([string(credentialsId: 'docker-pass', variable: 'DOCKER_PASS')]) {
                    bat 'docker login -u taiyeb008 -p %DOCKER_PASS%'
                    bat 'docker build -t portfolio-project:latest -f Dockerfile.dockerfile .'
                    bat 'docker logout'
                    bat 'docker save portfolio-project:latest -o portfolio-project.tar'
                }
                archiveArtifacts artifacts: 'portfolio-project.tar', allowEmptyArchive: true
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'npm test'
            }
        }
        stage('Code Quality Analysis') {
            tools {
                sonarqube 'SonarQube_Scanner'
            }
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    bat """
                    C:\\ProgramData\\Jenkins\\.jenkins\\tools\\hudson.plugins.sonar.SonarRunnerInstallation\\SonarQube_Scanner\\bin\\sonar-scanner -Dsonar.projectKey=portfolio-project -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.token=%SONAR_TOKEN%
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Replace 'C:\\path\\to\\deployment\\directory' with the actual deployment path
                bat 'xcopy /s /i /y .\\dist\\* C:\\path\\to\\deployment\\directory'
            }
        }
        stage('Release') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Releasing the application...'
                // Add your release steps here
            }
        }
        stage('Monitoring and Alerting') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Setting up monitoring and alerting...'
                // Add your monitoring steps here
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

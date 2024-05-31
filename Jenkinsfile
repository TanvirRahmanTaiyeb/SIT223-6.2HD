pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS' // Your NodeJS tool installation name
        // Assuming 'SonarQube_Scanner' is the name of your SonarQube Scanner installation
        sonar 'SonarQube_Scanner'
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
            }
        }

        stage('Docker Build and Save') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASS')]) {
                    bat 'docker login -u taiyeb008 -p %DOCKER_PASS%'
                    bat 'docker build -t portfolio-project:latest -f Dockerfile.dockerfile .'
                    bat 'docker logout'
                    bat 'docker save portfolio-project:latest -o portfolio-project.tar'
                }
                archiveArtifacts artifacts: 'portfolio-project.tar'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'npm test'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'sonar-scanner -Dsonar.projectKey=portfolio-project -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=%SONAR_TOKEN%'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                bat 'xcopy /s /i /y .\\dist\\* C:\\path\\to\\deployment\\directory'
            }
        }

        stage('Release') {
            steps {
                echo 'Releasing the application...'
                // Add your release steps here
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                echo 'Setting up monitoring and alerting...'
                // Add your monitoring and alerting steps here
            }
        }
    }
}

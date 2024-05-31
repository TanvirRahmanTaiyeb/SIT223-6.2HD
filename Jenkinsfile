pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // This should be the name you configured in Global Tool Configuration
        sonar 'SonarQube_Scanner' // This should be the name you configured in Global Tool Configuration
    }

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
                script {
                    def dockerImage = docker.build("portfolio-project:latest", "-f Dockerfile.dockerfile .")
                }
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube_Scanner') {
                    sh 'sonar-scanner -Dsonar.projectKey=portfolio-project -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'xcopy /s /i /y .\\dist\\* C:\\path\\to\\deployment\\directory'
            }
        }

        stage('Release') {
            steps {
                echo 'Releasing the application...'
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                echo 'Setting up monitoring and alerting...'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'portfolio-project.tar', fingerprint: true
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube'
    }

    tools {
        sonar 'DefaultScanner'
    }

    stages {
        stage('Debug') {
            steps {
                sh '''
                    echo "PATH is: $PATH"
                    which sonar-scanner || echo "Sonar-scanner no está disponible"
                '''
            }
        }

        stage('Checkout') {
            steps {
                git 'https://github.com/jram97/backend-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'sonar-scanner -X'
                }
            }
        }
    }
}


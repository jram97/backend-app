pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube' // Nombre del servidor configurado en Jenkins
    }

    tools {
        sonarQubeScanner 'DefaultScanner' // Este nombre debe coincidir
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jram97/backend-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'sonar-scanner'
                }
            }
        }
    }
}


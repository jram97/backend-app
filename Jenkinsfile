pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube' // Nombre del servidor configurado en Jenkins
    }

    tools {
        // Aquí debes poner el nombre exacto que le diste en Global Tool Config para SonarQube Scanner
        sonarRunner 'DefaultScanner'
    }

    stages {
        stage('Debug') {
            steps {
                sh 'which sonar-scanner || echo "Sonar-scanner no está disponible"'
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
                    // Usa la variable tool para apuntar al scanner instalado por Jenkins
                    sh "${tool 'DefaultScanner'}/bin/sonar-scanner"
                }
            }
        }
    }
}


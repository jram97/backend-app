pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube' // Cambialo si le diste otro nombre en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jram97/backend-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Poné aquí lo necesario para tu proyecto (opcional)
                // Por ejemplo: sh 'npm install' o 'pip install -r requirements.txt'
                echo 'Instalando dependencias...'
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

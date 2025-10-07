pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Prepare libs') {
            steps {
                sh '''
                    mkdir -p lib
                    curl -L -o lib/jakarta.servlet-api.jar https://repo1.maven.org/maven2/jakarta/servlet/jakarta.servlet-api/5.0.0/jakarta.servlet-api-5.0.0.jar
                '''
            }
        }

        stage('Compile') {
            steps {
                sh '''
                    ls -l lib
                    mkdir -p build/classes
                    javac -cp lib/jakarta.servlet-api.jar -d build/classes -sourcepath src/main/java $(find src/main/java -name "*.java")
                '''
            }
        }

        stage('Package WAR') {
            steps {
                sh '''
                    mkdir -p build/webapp/WEB-INF/classes
                    mkdir -p build/webapp/WEB-INF/lib
                    # Copia las clases compiladas
                    cp -r build/classes/* build/webapp/WEB-INF/classes/
                    # Copia el servlet-api.jar (opcional, usualmente el servidor lo provee)
                    cp lib/jakarta.servlet-api.jar build/webapp/WEB-INF/lib/
                    # Copia el contenido web (JSP, HTML, etc)
                    cp -r src/main/webapp/* build/webapp/
                    # Empaqueta el WAR
                    cd build/webapp
                    jar -cvf ../../myapp.war *
                    cd ../..
                '''
            }
        }

        stage('SonarQube Analysis') {
	  steps {
	    withSonarQubeEnv('SonarQube') {
	      sh 'sonar-scanner -Dsonar.projectKey=backend-app -Dsonar.sources=src'
	    }
	  }
	}

    }

    post {
        success {
            echo 'Build and package completed successfully!'
            archiveArtifacts artifacts: 'myapp.war', fingerprint: true
        }
        failure {
            echo 'Build failed!'
        }
    }
}


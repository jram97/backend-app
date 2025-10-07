pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

	stage('Compile') {
	    steps {
		sh '''
		    mkdir -p build/classes
		    javac -cp lib/jakarta.servlet-api.jar -d build/classes -sourcepath src/main/java $(find src/main/java -name "*.java")
		'''
	    }
	}

	stage('Package WAR') {
	    steps {
		sh '''
		    mkdir -p build/webapp/WEB-INF/classes
		    cp -r build/classes/* build/webapp/WEB-INF/classes/
		    cp libs/jakarta.servlet-api.jar build/webapp/WEB-INF/lib/
		    cp -r src/main/webapp/* build/webapp/
		    cd build/webapp
		    jar -cvf ../../myapp.war *
		    cd ../..
		'''
	    }
	}

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=mi-proyecto \
                          -Dsonar.sources=src/main/java \
                          -Dsonar.java.binaries=build/classes
                    '''
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/pahanender/jenkins-test-python-app.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    scannerHome = tool 'SonarQubeScanner'
                }
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}

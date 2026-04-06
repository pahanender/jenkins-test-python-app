pipeline {
    agent any

    environment {
        SONAR_HOST = 'http://95.174.93.5:9000'
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Клонируем репозиторий...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📥 Установка зависимостей...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app python:3.11-slim \
                        sh -c 'if [ -f "requirements.txt" ]; then pip3 install -r requirements.txt; else echo "Flask==2.3.0" > requirements.txt && pip3 install -r requirements.txt; fi'
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '🔍 Анализ кода в SonarQube...'
                sh '''
                    # Скачиваем sonar-scanner
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                    unzip -q sonar-scanner.zip
                    rm sonar-scanner.zip
                    
                    # Запускаем анализ с ЯВНОЙ передачей токена
                    ./sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner \
                        -Dsonar.projectKey=jenkins-python-app \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_HOST} \
                        -Dsonar.token=${SONAR_TOKEN} \
                        -Dsonar.python.version=3.11 \
                        -Dsonar.projectName="Jenkins Python App"
                '''
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Сборка #${BUILD_NUMBER} завершена!'
                sh 'ls -la'
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline finished'
            cleanWs()
        }
        failure {
            echo '❌ Pipeline failed - check logs'
        }
    }
}

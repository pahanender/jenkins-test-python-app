pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Клонируем репозиторий...'
                checkout scm
            }
        }

        stage('Python Setup') {
            steps {
                echo '🐍 Python версия:'
                sh 'python3 --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📥 Установка зависимостей...'
                sh '''
                    if [ -f "requirements.txt" ]; then
                        pip3 install -r requirements.txt
                    else
                        echo "Flask" > requirements.txt
                        pip3 install -r requirements.txt
                    fi
                '''
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Сборка завершена!'
                echo "Build #${BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline finished'
            cleanWs()
        }
    }
}

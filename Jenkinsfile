pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Репозиторий уже клонирован'
                // checkout делается автоматически в declarative pipeline
            }
        }

        stage('Python Setup') {
            // 🐍 Запускаем эту стадию в контейнере с Python!
            agent {
                docker { 
                    image 'python:3.11-slim'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo '🐍 Python версия:'
                sh 'python3 --version'
                sh 'pip3 --version'
            }
        }

        stage('Install Dependencies') {
            agent {
                docker { 
                    image 'python:3.11-slim'
                }
            }
            steps {
                echo '📥 Установка зависимостей...'
                sh '''
                    if [ -f "requirements.txt" ]; then
                        pip3 install -r requirements.txt
                    else
                        echo "Flask==2.3.0" > requirements.txt
                        pip3 install -r requirements.txt
                    fi
                '''
            }
        }

        stage('Build') {
            agent {
                docker { 
                    image 'python:3.11-slim'
                }
            }
            steps {
                echo '🔨 Сборка завершена!'
                echo "Build #${BUILD_NUMBER}"
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

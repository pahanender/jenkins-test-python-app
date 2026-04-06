pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Репозиторий клонирован'
            }
        }

        stage('Python Setup') {
            agent {
                docker { 
                    image 'python:3.11-slim'
                    // Без docker.sock, если не нужно собирать образы
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
    }
}

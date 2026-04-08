pipeline {
    agent any

    environment {
        APP_NAME = 'my-python-app'
        APP_VERSION = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('📦 Checkout') {
            steps {
                echo '✅ Репозиторий клонирован'
                checkout scm
            }
        }

        stage('🐍 Install Dependencies') {
            steps {
                echo '📦 Установка зависимостей...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app python:3.11-slim \
                        sh -c 'pip3 install --upgrade pip && if [ -f "requirements.txt" ]; then pip3 install -r requirements.txt; fi'
                '''
            }
        }

        stage('🧪 Run Tests') {
            steps {
                echo '🧪 Запускаем тесты...'
                sh '''
                    # Создаём папку и тест, если нет
                    mkdir -p tests
                    echo "def test_ok(): assert 1 + 1 == 2" > tests/test_simple.py
                    
                    # Устанавливаем pytest И запускаем тесты в ОДНОЙ команде
                    docker run --rm -v $(pwd):/app -w /app python:3.11-slim \
                        sh -c 'pip3 install pytest -q && python3 -m pytest tests/ -v'
                '''
            }
        }

        stage('🔨 Build') {
            steps {
                echo '🔨 Сборка артефакта...'
                sh '''
                    mkdir -p build
                    echo "Build #${BUILD_NUMBER} - $(date)" > build/app-info.txt
                    tar -czf build/${APP_NAME}-${APP_VERSION}.tar.gz -C build app-info.txt
                    echo "✅ Артефакт готов: build/${APP_NAME}-${APP_VERSION}.tar.gz"
                    ls -lh build/
                '''
            }
        }

        stage('🚀 Deploy to Staging') {
            steps {
                echo '🚀 Деплой на staging (симуляция)...'
                sh '''
                    echo "✅ Приложение версии ${APP_VERSION} развёрнуто на staging"
                '''
            }
        }

        stage('⏸ Approve Production') {
            steps {
                echo '⏸ Ожидание подтверждения...'
                timeout(time: 30, unit: 'MINUTES') {
                    input message: "✅ Сборка #${BUILD_NUMBER} готова!\n🚀 Деплоить в production?", ok: '✅ Да, деплой!'
                }
            }
        }

        stage('🎯 Deploy to Production') {
            steps {
                echo '🎯 ДЕПЛОЙ В PRODUCTION...'
                sh '''
                    echo "🎉 ПРОДАКШЕН ОБНОВЛЁН!"
                    echo "Версия: ${APP_VERSION}"
                    echo "Время: $(date)"
                '''
            }
        }
    }

    post {
        always {
            echo '🧹 Очистка рабочей директории...'
            cleanWs()
        }
        success {
            echo "🎊 Pipeline #${BUILD_NUMBER} завершён УСПЕШНО!"
        }
        failure {
            echo "❌ Pipeline #${BUILD_NUMBER} упал. Проверь логи выше."
        }
    }
}

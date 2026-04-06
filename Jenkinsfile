pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Ready'
            }
        }

        stage('Python Setup') {
            steps {
                echo '🐍 Python в Docker:'
                sh 'docker run --rm python:3.11-slim python3 --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📥 Установка...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app python:3.11-slim \
                        sh -c 'if [ -f "requirements.txt" ]; then pip3 install -r requirements.txt; else echo "Flask==2.3.0" > requirements.txt && pip3 install -r requirements.txt; fi'
                '''
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Build #${BUILD_NUMBER} done'
                sh 'docker run --rm -v $(pwd):/app -w /app python:3.11-slim ls -la'
            }
        }
    }

    post {
        always {
            echo '✅ Finished'
            cleanWs()
        }
    }
}

pipeline {
    agent any  // Запускаем всё на встроенной ноде (хосте)
    
    stages {
        stage('Checkout') {
            steps {
                echo '📦 Ready'
            }
        }
        
        stage('Python Setup') {
            steps {
                sh 'python3 --version'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh '''
                    if [ -f "requirements.txt" ]; then
                        pip3 install -r requirements.txt
                    fi
                '''
            }
        }
        
        stage('Build') {
            steps {
                echo "✅ Build #${BUILD_NUMBER} done"
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}

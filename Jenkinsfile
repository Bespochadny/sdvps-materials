pipeline {
    agent any

    environment {
        APP_NAME = 'myapp'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Tests') {
            steps {
                sh 'go test . -v'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${APP_NAME}:${IMAGE_TAG} ."
            }
        }
    }

    post {
        success {
            echo '✅ Сборка успешно завершена!'
        }
        failure {
            echo '❌ Сборка упала. Проверьте логи.'
        }
        always {
            cleanWs()
        }
    }
}

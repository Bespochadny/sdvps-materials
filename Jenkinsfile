pipeline {
    agent any

    environment {
        // Замените на реальный IP вашей виртуальной машины, где стоит Nexus
        // Если Jenkins и Nexus на одной машине, используйте localhost или 127.0.0.1
        NEXUS_URL = 'http://81.26.177.89:8081' 
        REPO_NAME = 'raw-releases'
        APP_NAME = 'my-go-app'
        
        // Логин и пароль Nexus (admin / пароль)
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin12345'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Binary') {
            steps {
                echo '🔨 Компилируем Go файл...'
                // Команда из Dockerfile обычно выглядит так:
                sh 'go build -o ${APP_NAME} .'
                
                // Проверим, что файл создался
                sh 'ls -lh'
            }
        }

        stage('Upload to Nexus') {
            steps {
                echo 'Загружаем бинарник в Nexus...'
                
                // Формируем URL для загрузки
                // Формат: http://host/repository/repo-name/filename
                sh """
                    curl -u "${NEXUS_USER}:${NEXUS_PASS}" \
                    --upload-file ${APP_NAME} \
                    ${NEXUS_URL}/repository/${REPO_NAME}/${APP_NAME}
                """
                
                echo 'Загрузка завершена!'
            }
        }
    }

    post {
        success {
            echo 'Сборка и загрузка прошли успешно!'
        }
        failure {
            echo 'Ошибка в процессе.'
        }
        always {
            cleanWs()
        }
    }
}

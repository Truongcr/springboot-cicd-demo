pipeline {
    agent any

    environment {
        IMAGE_NAME = "spring-demo"
        CONTAINER_NAME = "spring-demo-container"
        HOST_PORT = "8087"          // cổng host muốn expose
        APP_PORT = "8080"           // cổng app bên trong container (mặc định Spring Boot)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Truongcr/springboot-cicd-demo.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d -p ${HOST_PORT}:${APP_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "🚀 Ứng dụng đã build & deploy thành công! Truy cập: http://localhost:${HOST_PORT}/hello"
        }
        failure {
            echo "❌ Build hoặc Deploy thất bại!"
        }
    }
}

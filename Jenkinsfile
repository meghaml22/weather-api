pipeline {
    agent any

    environment {
        IMAGE_NAME = "weather-api:latest"
        CONTAINER_NAME = "weather-api-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/meghaml22/weather-api.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", ".")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    bat 'docker run --rm ${IMAGE_NAME} pytest tests/test_app.py'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove any existing container
                    bat "docker stop ${CONTAINER_NAME} || true"
                    bat "docker rm ${CONTAINER_NAME} || true"

                    // Run new container
                    bat "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}"
                }
            }
        }
    }
}

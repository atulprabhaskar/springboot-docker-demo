pipeline {
    agent any

    environment {
        IMAGE_NAME = 'springboot-docker-demo'
        IMAGE_TAG = 'latest'
        DOCKER_REGISTRY = 'localhost:5000' // use Nexus Docker registry or DockerHub
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/atulprabhaskar/springboot-docker-demo.git', branch: 'main'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to Docker Registry') {
            when {
                expression { return DOCKER_REGISTRY != '' }
            }
            steps {
                script {
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy (Optional)') {
            steps {
                sh '''
                docker rm -f springboot-app || true
                docker run -d --name springboot-app -p 8080:8080 ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}
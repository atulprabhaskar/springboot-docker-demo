pipeline {
    agent any

    environment {
        DOCKER_HOST = 'tcp://192.168.56.113:2375'
        DOCKER_IMAGE = 'springboot-docker-demo:latest'
        CONTAINER_NAME = 'springboot-demo'
    }

    options {
        timestamps()
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '🔄 Checking out source code...'
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                echo '📦 Running Maven clean package...'
                sh 'mvn clean package -DskipTests=false'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image on remote host...'
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        sh "docker build -t ${DOCKER_IMAGE} ."
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🚀 Running Docker container on remote host...'
                script {
                    docker.withServer("${DOCKER_HOST}") {
                        sh """
                            docker rm -f ${CONTAINER_NAME} || true
                            docker run -d -p 9000:8080 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }

        stage('Push to Docker Registry (optional)') {
            when {
                expression { return false } // enable later if pushing images
            }
            steps {
                echo "📤 Pushing image to registry (placeholder)..."
                // Example: sh "docker push my-registry/${DOCKER_IMAGE}"
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo '📋 Pipeline finished.'
        }
    }
}

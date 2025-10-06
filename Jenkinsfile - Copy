pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'springboot-docker-demo:latest'
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
                echo "🐳 Building Docker image: ${env.DOCKER_IMAGE}"
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push to Docker Registry (optional)') {
            when {
                expression { return false } // Change to true if you plan to push images
            }
            steps {
                echo "📤 Pushing image to registry (placeholder)..."
                // Example: sh 'docker push my-registry/${DOCKER_IMAGE}'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🚀 Running Docker container...'
                sh '''
                    docker rm -f springboot-demo || true
                    docker run -d -p 9000:8080 --name springboot-demo springboot-docker-demo:latest
                '''
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

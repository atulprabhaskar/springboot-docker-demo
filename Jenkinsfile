pipeline {
    agent any

    tools {
        maven 'Maven3'   // Make sure Maven3 is installed in Jenkins (Manage Jenkins → Global Tool Config)
        jdk 'JDK17'      // Ensure JDK 17 is configured in Jenkins (same place)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/atulprabhaskar/springboot-docker-demo.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t springboot-docker-demo:latest .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 8080:8080 springboot-docker-demo:latest'
                }
            }
        }
    }
}
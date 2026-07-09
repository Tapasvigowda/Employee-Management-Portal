pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-app"
        CONTAINER_NAME = "springboot-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: ''https://github.com/DeepikaCAshok/Employee-Management-Portal.git
            }
        }

        stage('Build Spring Boot Application') {
            steps {
                sh '''
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh '''
                docker compose down --remove-orphans || true
                '''
            }
        }

        stage('Deploy Using Docker Compose') {
            steps {
                sh '''
                docker compose up -d --build
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                docker ps
                sleep 10
                curl -f http://localhost:8083/employees
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully'
        }

        failure {
            echo 'Deployment failed. Check Jenkins logs.'
        }

        always {
            sh '''
            docker ps
            '''
        }
    }
}

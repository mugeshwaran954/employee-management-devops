pipeline {
    agent any

    tools {
        maven 'Maven 3' // Ensure this matches your configured Maven tool in Jenkins
    }

    environment {
        IMAGE_NAME = 'employee-management'
        IMAGE_TAG = '1.0'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/mugeshwaran954/employee-management-devops.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Optional: stop and remove any existing container
                sh 'docker rm -f employee-app || true'
                // Run the new container on port 8081
                sh 'docker run -d -p 8081:8080 --name employee-app $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Docker container created successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}

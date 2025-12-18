pipeline {
    agent any

    environment {
        APP_NAME = "chat-app-backend"
        DOCKER_IMAGE = "chat-app-backend:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ayush-Bajagain/chat-application-backend.git'
            }
        }

        stage('Build Jar') {
            steps {
                // Use Jenkins-managed Maven
                withMaven(maven: 'maven') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $APP_NAME || true
                docker rm $APP_NAME || true
                docker run -d \
                  --name $APP_NAME \
                  -p 8888:8888 \
                  $DOCKER_IMAGE
                '''
            }
        }
    }
}

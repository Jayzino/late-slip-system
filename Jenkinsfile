pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t late-slip:${BUILD_NUMBER} .
                docker tag late-slip:${BUILD_NUMBER} late-slip:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                (docker ps -q --filter "name=late-slip" && docker stop late-slip && docker rm late-slip) || true
                docker run -d --name late-slip -p 80:80 late-slip:latest
                '''
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'curl -fsS http://localhost/ | head -n 5'
            }
        }
    }
}





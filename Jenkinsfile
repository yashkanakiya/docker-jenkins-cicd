pipeline {
    agent any
    stages {
         stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f flask-app || true
                docker run -d -p 5000:5000 flask-app --name flask-app
                '''
            }
        }
    }
}
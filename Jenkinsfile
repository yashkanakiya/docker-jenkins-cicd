pipeline {
    agent none  
    
    stages {
        stage('Checkout Code') {
            agent any  
            steps {
                checkout scm
            }
        }
        stage('Build and Run') {
            agent {
                label 'built-in'  
            }
            steps {
                sh '''
                docker build -t flask-app .
                docker rm -f flask-app || true
                docker run -d -p 5000:5000 --name flask-app flask-app
                '''
            }
        }
    }
}
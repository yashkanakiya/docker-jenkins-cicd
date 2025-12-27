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
                sh 'docker build -t flask-app:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f flask-app || true
                docker run -d \
                  --name flask-app \
                  -p 5000:5000 \
                  flask-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo "Flask app deployed successfully 🚀"
        }
        failure {
            echo "Build failed ❌"
        }
    }
}

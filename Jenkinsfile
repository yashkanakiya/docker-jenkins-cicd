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
                script {
                    dockerImage = docker.build("flask-app:latest")
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
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
    }

    post {
        success {
            echo "Application deployed successfully 🚀"
        }
        failure {
            echo "Build failed ❌"
        }
    }
}

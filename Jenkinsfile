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
                    dockerImage = docker.build("flask-app:${BUILD_ID}")
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    // Stop and remove if exists
                    sh 'docker rm -f flask-app || true'
                    
                    // Run new container
                    dockerImage.run(
                        "--name flask-app -p 5000:5000 -d"
                    )
                }
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up...'
            sh 'docker rm -f flask-app || true'
        }
    }
}
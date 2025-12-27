pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f flask-container || true
                docker run -d -p 5000:5000 --name flask-container flask-app
                '''
            }
        }
    }
}


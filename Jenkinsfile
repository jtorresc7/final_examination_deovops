pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-html-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f my-container || true'
                sh 'docker run -d -p 8080:80 --name my-container my-html-app'
            }
        }
    }
}

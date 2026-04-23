#pipeline {
#    agent any

#    stages {
#        stage('Build Docker Image') {
#            steps {
#                sh 'docker build -t my-html-app .'
#            }
#        }

#        stage('Run Container') {
#            steps {
#                sh 'docker rm -f my-container || true'
#                sh 'docker run -d -p 8081:80 --name my-container my-html-app'
#            }
#        }
#    }
#}


pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jtorresc7/my-html-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                sh 'docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f my-container || true'
                sh 'docker run -d -p 8081:80 --name my-container $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}

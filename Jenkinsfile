pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bayetalla/backend-django:latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry(
                    [credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/']
                ) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}

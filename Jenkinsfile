// pipeline {
//     agent any

//     environment {
//         DOCKER_IMAGE = "bayetalla/backend-django:latest"
//     }

//     stages {

//         stage('Build Docker Image') {
//             steps {
//                 sh 'docker build -t $DOCKER_IMAGE .'
//             }
//         }

//         stage('Push to Docker Hub') {
//             steps {
//                 withDockerRegistry(
//                     [credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/']
//                 ) {
//                     sh 'docker push $DOCKER_IMAGE'
//                 }
//             }
//         }
//     }
// }


pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bayetalla/backend-django:latest"
        // On récupère le chemin du scanner configuré dans "Tools"
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // 'SonarQube' doit correspondre au nom dans Manage Jenkins -> System
                withSonarQubeEnv('SonarQube') {
                    sh """
                    ${SCANNER_HOME}/bin/sonar-scanner \
                      -Dsonar.projectKey=backend-ci \
                      -Dsonar.sources=.
                    """
                }
            }
        }

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

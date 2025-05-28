pipeline {
    agent any

    environment {
        IMAGE_NAME = 'navyushrana/php-fullstack-app'
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
    }

    stages {
        stage('Build & Push PHP Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove any existing containers
                    bat 'docker-compose down'

                    // Wait a bit to let the network and volumes clean up

                    // Bring containers up using docker-compose
                    bat 'docker-compose up -d --build --remove-orphans'
                }
            }
        }
    }
}


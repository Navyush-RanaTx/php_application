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
                    bat 'docker-compose -f docker-compose.yml down'

                    // Wait a bit to let the network and volumes clean up
                    bat 'timeout /t 5'

                    // Bring containers up using docker-compose
                    bat 'docker-compose -f docker-compose.yml up -d --build --remove-orphans'
                }
            }
        }
    }
}


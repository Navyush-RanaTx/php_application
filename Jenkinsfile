pipeline {
    agent any

    environment {
        IMAGE_NAME = 'navyushrana/php-fullstack-app'
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
    }

    stages {
        // stage('Checkout') {
        //     steps {
        //         git 'https://github.com/GoutamTx/php_application.git'
        //     }
        // }

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
                    bat 'docker swarm init || exit 0'
                    bat 'docker stack rm php-app'
                    bat 'timeout /t 10'
                    bat 'docker stack deploy -c docker-compose.yml php-app'
                }
            }
         }
    }


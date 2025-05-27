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
                bat '''
                    docker-compose -f docker-compose.yml down
                    timeout /t 10
                    docker-compose -f docker-compose.yml up -d --remove-orphans --build

                '''
            }
        }
    }
}


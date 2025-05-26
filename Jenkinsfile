pipeline {
    agent any

    environment {
        IMAGE_NAME = 'goutam24/php-fullstack-app'
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
    }

    stages {
      //  stage('Checkout') {
        //    steps {
          //      git 'https://github.com/GoutamTx/php_application.git'
            //}
        //}

        stage('Build & Push PHP Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-creds') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    docker stack rm php-app || echo "Stack not found, continuing"
                    timeout 10
                    docker stack deploy -c docker-compose.yml php-app
                '''
            }
        }
    }
}

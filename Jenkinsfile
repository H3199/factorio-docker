pipeline {
    agent any
    environment {
        COMPOSE_FILE = "docker-compose.yml"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy with Docker-Compose') {
            steps {
                script {
                    sh """
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Factorio server deployed successfully with Docker Compose!'
        }
        failure {
            echo 'Deployment failed. Check the logs for details.'
        }
    }
}

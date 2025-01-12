pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "factorio-server:latest"       // Image name for the Factorio server
        CONTAINER_NAME = "factorio-server-container" // Name of the running container
        SERVER_PORT = "34197"                        // Factorio server port
        SERVER_VOLUME = "/home/eero/factorio-server" // Volume for the Factorio server
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the repository's Dockerfile
                    sh """
                    docker build -t ${DOCKER_IMAGE} .
                    """
                }
            }
        }
        stage('Deploy Factorio Server') {
            steps {
                script {
                    // Stop and remove the old container if it exists
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """

                    // Run the Factorio server container
                    sh """
                    docker run -d --name ${CONTAINER_NAME} \
                    -p ${SERVER_PORT}:${SERVER_PORT}/udp \
                    -v ${SERVER_VOLUME}:/factorio \
                    ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Factorio server deployed successfully!'
        }
        failure {
            echo 'Build or deployment failed. Check logs for details.'
        }
    }
}

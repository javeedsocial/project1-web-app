pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // SCM polling every 5 minutes
    }

    environment {
        DOCKER_IMAGE = 'your-docker-image-name'
        CONTAINER_NAME = 'web-server-container'
        PORT_MAPPING = '8080:80'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Check out your source code from version control (e.g., Git)
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Copy server.js into the Docker image
                    sh 'cp server.js .'
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}", '.')
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the container
                    docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").withRun("--name ${CONTAINER_NAME} -p ${PORT_MAPPING}")
                }
            }
        }
    }
}

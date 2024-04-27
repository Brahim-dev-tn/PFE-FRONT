def DOCKER_IMAGE_NAME = "app-front"
def DOCKER_IMAGE_VERSION = "1.0.0"

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                deleteDir()  // Cleans the workspace
                echo 'Workspace cleaned successfully.'
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
                echo 'Code checked out successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${env.DOCKER_IMAGE} ."
                    echo "Docker image built: ${env.DOCKER_IMAGE}"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker tag ${env.DOCKER_IMAGE} ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"
                        sh "docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"
                        echo "Docker image pushed: ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"
                    }
                }
            }
        }

        stage('Remove Docker Compose Containers') {
            steps {
                sh 'docker-compose down'
                echo 'Docker compose containers are down.'
            }
        }

        stage('Start Docker Compose') {
            steps {
                sh 'docker-compose up -d'
                echo 'Docker compose started in detached mode.'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

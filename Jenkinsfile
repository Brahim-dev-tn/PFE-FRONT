def DOCKER_IMAGE_NAME = "app-front"
def DOCKER_IMAGE_VERSION = "1.0.0"

pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "${DOCKER_IMAGE_NAME}"
        DOCKER_IMAGE_VERSION = "${DOCKER_IMAGE_VERSION}"
    }
        


        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION} ."
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    sh "docker login -u brahimbenyouns@gmail.com -p Lifeisgoodbrahim@@"
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION} \$DOCKER_HUB_USERNAME/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"
                    sh "docker push \$DOCKER_HUB_USERNAME/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_VERSION}"

                }
            }
        }


        stage('Remove Docker Compose Containers') {
            steps {
                sh 'docker-compose down'
            }
        }

        stage('Start Docker Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

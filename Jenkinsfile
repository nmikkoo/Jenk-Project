pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'your-dockerhub-credentials-id'
        DOCKER_IMAGE_NAME = 'your-dockerhub-username/your-image-name'
    }

    stages {
        stage('Your Name - Build Docker Image') {
            steps {
                script {
                    // Define the image name with a tag
                    def imageName = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    
                    // Build the Docker image
                    sh "docker build -t ${imageName} ."
                }
            }
        }

        stage('Your Name - Login to Dockerhub') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        // No additional steps required here
                    }
                }
            }
        }

        stage('Your Name - Push image to Dockerhub') {
            steps {
                script {
                    // Define the image name with a tag
                    def imageName = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
    }
}

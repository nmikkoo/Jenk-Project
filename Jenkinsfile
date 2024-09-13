pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials' // DockerHub credentials ID
        GITHUB_CREDENTIALS_ID = 'GitHub_Credentials' // GitHub credentials ID
        DOCKER_IMAGE_NAME = 'nats000/jenkPro' // DockerHub username and image name
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                script {
                    // Clean up the existing directory if it exists
                    sh 'rm -rf Jenk-Project'

                    // Clone the repository using GitHub credentials
                    withCredentials([usernamePassword(credentialsId: "${GITHUB_CREDENTIALS_ID}", usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh 'git clone https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nmikkoo/Jenk-Project'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Define the image name with a tag
                    def imageName = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    
                    // Build the Docker image
                    sh "docker build -t ${imageName} ."
                }
            }
        }

        stage('Login to Dockerhub') {
            steps {
                script {
                    // Log in to Docker Hub using Docker credentials
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        // No additional steps required here
                    }
                }
            }
        }

        stage('Push image to Dockerhub') {
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

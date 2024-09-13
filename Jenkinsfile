pipeline {
    agent any

    environment {
        // Use the correct credentials IDs you specified
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'  // DockerHub credentials ID
        GITHUB_CREDENTIALS_ID = 'GitHub_Credentials'     // GitHub credentials ID
        DOCKER_IMAGE_NAME = 'nats000/jenkPro'             // Replace with your actual DockerHub username and image name
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                script {
                    // Example usage of GitHub credentials if you need to clone a private repo
                    withCredentials([usernamePassword(credentialsId: "${GITHUB_CREDENTIALS_ID}", usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        // Replace 'your-username/your-repo' with your actual GitHub repository details
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
                        echo 'Logged in successfully.'
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

pipeline {
    agent any

    environment {
        // Use the correct credentials IDs you specified
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'        
        GITHUB_CREDENTIALS_ID = 'GitHub_Credentials'           
        DOCKER_IMAGE_NAME = 'nats000/jenkPro' 
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${GITHUB_CREDENTIALS_ID}", usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh 'git clone https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/nmikkoo/Jenk-Project'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    
                    sh "docker build -t ${imageName} ."
                }
            }
        }

        stage('Login to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                    }
                }
            }
        }

        stage('Push image to Dockerhub') {
            steps {
                script {
                    def imageName = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
    }
}

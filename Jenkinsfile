pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'khushboo289/flaskapp'
        DOCKER_HUB_CREDENTIALS = 'dockerhub-login'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: ''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_CREDENTIALS}") {
                        echo "Logged in to Docker Hub"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_CREDENTIALS}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image built and pushed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}

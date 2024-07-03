pipeline {
    agent any

    environment {
        registry = "amrit96/snake"
        registryCredential = 'training_creds'
    }

    stages {
        stage('Cloning Git') {
            steps {
                // Clone the repository
                checkout scm
            }
        }
        
        stage('Build-and-Tag') {
            steps {
                script {
                    // Build the Docker image
                    app = docker.build("${registry}")
                }
            }
        }
        
        stage('Post-to-dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${registryCredential}") {
                        // Push the Docker image to Docker Hub
                        app.push("latest")
                    }
                }
            }
        }
        
        stage('Pull-image-server') {
            steps {
                script {
                    // Pull the Docker image and restart the containers using Docker Compose
                    sh "docker-compose down"
                    sh "docker-compose up -d"
                }
            }
        }
    }
}


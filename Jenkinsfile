pipeline {
    agent any
    environment {
        TAG_NUMBER = 0 // Initialize TAG_NUMBER to 0
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                // Increment the tag number for each build
                script {
                    TAG_NUMBER = TAG_NUMBER + 1
                }

                // Build the Docker image with the updated tag number
                sh "docker-compose build --build-arg TAG_NUMBER=${TAG_NUMBER}"

                // Update the .env file with the new tag number
                sh "echo 'TAG_NUMBER=${TAG_NUMBER}' > .env"
            }
        }
        stage('Push') {
            steps {
                echo 'Pushing the Docker image to Docker Hub...'
                docker.withRegistry(url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub') {
                    sh "docker-compose push"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Pulling and running the Docker container...'
                // Deploy the Docker container using the updated tag number
                sh "docker-compose pull"
                sh "docker-compose up -d"
            }
        }
    }
}


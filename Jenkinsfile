pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                // Increment the tag number for each build
                sh 'TAG_NUMBER=$(($TAG_NUMBER + 1))'

                // Build the Docker image with the updated tag number
                sh "docker-compose build --build-arg TAG_NUMBER=${TAG_NUMBER}"

                // Update the .env file with the new tag number
                sh "echo 'TAG_NUMBER=${TAG_NUMBER}' > .env"
                
                // Add a sleep of 1 minute (for example, you can adjust the duration as needed)
                sleep(time: 1, unit: 'MINUTES')
            }
        }
        stage('Push') {
            steps {
                echo 'Pushing the Docker image to Docker Hub...'
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                    sh "docker login -u ankit191919 -p ${dockerhub}"
                    // Push the Docker image with the corresponding tag
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


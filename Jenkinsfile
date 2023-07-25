pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from GitHub
                git credentialsId: 'Git', url: 'https://github.com/Ankitsrivastav19/devops-simplilearn.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                // Build Docker image and push to Docker Hub
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        def appImage = docker.build("ankit191919/docker-image:${env.BUILD_ID}")
                        appImage.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Pull and run the Docker image
                sh 'docker run -d -p 8080:80 ankit191919/docker-image:${env.BUILD_ID}'
            }
        }
    }
}

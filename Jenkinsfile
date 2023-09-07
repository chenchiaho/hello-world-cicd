pipeline {
    agent any 

    tools {
        maven 'maven name'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'  // Use 'bat' instead of 'sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "my-tomcat-app"
                    def imageTag = "latest"

                    // Building Docker image using 'bat'
                    bat "docker build -t ${imageName}:${imageTag} ."
                }
            }
        }

        stage('Deploy to Docker') {
            steps {
                script {
                    def containerName = "my-running-app"

                    // Stopping existing containers and running new one using 'bat'
                    bat "docker stop ${containerName} || echo 'No container to stop'"
                    bat "docker rm ${containerName} || echo 'No container to remove'"
                    bat "docker run -d --name ${containerName} -p 8080:8080 my-tomcat-app:latest"
                }
            }
        }
    }
}

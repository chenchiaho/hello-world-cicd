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

                    // Check if the container is running
                    def isRunning = bat(script: "docker ps -q -f name=${containerName}", returnStatus: true)
                    if (isRunning == 0) {
                        // Stop and remove the container
                        bat "docker rm -f ${containerName} || echo 'Failed to stop and remove container'"
                    } else {
                        echo 'No container to stop and remove'
                    }

                    // Run the new container
                    bat "docker run -d --name ${containerName} -p 8090:8090 my-tomcat-app:latest"
                }
            }
        }
    }
}

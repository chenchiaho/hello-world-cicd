pipeline {
    agent any 

    tools {
        maven 'Maven 3.2.3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // This checks out the code from the configured source code management in Jenkins (e.g., Git).
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'  // This will produce the .war file in the target directory.
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Constructing the Docker image name and tag
                    def imageName = "my-tomcat-app"
                    def imageTag = "latest"

                    // Building the Docker image using the Dockerfile in the workspace.
                    sh "docker build -t ${imageName}:${imageTag} ."
                }
            }
        }

        stage('Deploy to Docker') {
            steps {
                script {
                    def containerName = "my-running-app"

                    // Stopping any existing containers with the same name (this is a simplistic approach)
                    sh "docker stop ${containerName} || true && docker rm ${containerName} || true"

                    // Running the Docker container
                    sh "docker run -d --name ${containerName} -p 8080:8080 my-tomcat-app:latest"
                }
            }
        }
    }
}

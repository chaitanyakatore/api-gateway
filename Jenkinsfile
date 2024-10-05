pipeline {
    agent any
    tools {
        maven 'my-maven'
        jdk 'my-jdk'
    }
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/chaitanyakatore/api-gateway.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                bat "mvn clean install -DskipTests"
            }
        }
        stage('Test') {
            steps {
                bat "mvn test"
            }
        }
        stage('Deploy') {
            steps {
                // Remove existing container and image if they exist
                bat "docker rm -f api-gateway-container || true"
                bat "docker rmi -f api-gateway-image || true"

                // Build and run the new container
                bat "docker build -t api-gateway-image ."
                bat "docker run -p 8060:8060 -d --name api-gateway-container api-gateway-image"
            }
        }
    }
}

pipeline {
    agent any

    tools {
        maven 'Maven 3.x'
        jdk 'Java 17'
    }

    environment {
        IMAGE_NAME = "my-spring-boot-app"
        CONTAINER_NAME = "spring-boot-container"
        HOST_PORT = "8081"
        CONTAINER_PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Docker Remove Old Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Docker Deploy / Run') {
            steps {
                sh "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}:latest"
                echo "Application deployed successfully on port ${HOST_PORT}!"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}


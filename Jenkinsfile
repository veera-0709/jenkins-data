pipeline {
    agent any

    tools {
        maven 'mvn-3.9.16'
        jdk 'JDK-21' 
    }

    environment {
        IMAGE_NAME = "my-spring-boot-app"
        CONTAINER_NAME = "spring-boot-container"
        APP_PORT = "8081"
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
                sh "docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} ${IMAGE_NAME}:latest"
                echo "Java 21 Application deployed completely on port ${APP_PORT}!"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

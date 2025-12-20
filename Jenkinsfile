pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "saifrehman123/flask-todo"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build \
                -t ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                flask-todo-app
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh """
                echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login \
                -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                docker push ${DOCKER_IMAGE}:latest
                """
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}


pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-todo"
        DOCKER_REPO = "saifrehman123"   // change if needed
        IMAGE_TAG  = "${BUILD_NUMBER}"
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
                -t ${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG} \
                flask-todo-app
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                docker push ${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
                docker logout
                """
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully: ${DOCKER_REPO}/${IMAGE_NAME}:${IMAGE_TAG}"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "kavyaamrutha221"
        ROLL_NUMBER       = "2023BCS0221"
        FRONTEND_IMAGE    = "${DOCKERHUB_USERNAME}/${REGISTER_NUMBER}_${ROLL_NUMBER}_frontend"
        BACKEND_IMAGE     = "${DOCKERHUB_USERNAME}/${REGISTER_NUMBER}_${ROLL_NUMBER}_backend"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t $FRONTEND_IMAGE ./frontend'
                sh 'docker build -t $BACKEND_IMAGE  ./backend'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $FRONTEND_IMAGE'
                    sh 'docker push $BACKEND_IMAGE'
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerID') // Your Jenkins credentials ID
        IMAGE_NAME = "preethinpatil/my_webapp"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/PNP-RIT/mywebapp',
                    branch: 'main',
                    credentialsId: 'dockerID'
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                     docker.withRegistry('https://index.docker.io/v1/', 'dockerhubID') {
                        dockerImage.push()

                    }
                }
            }
        }
    
    }
    post {
        always {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

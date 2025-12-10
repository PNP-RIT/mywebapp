pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')   // ID from Jenkins
        DOCKERHUB_USER = "preethinpatil"
        APP_NAME = "my_webapp"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git(
                    url: 'git@github.com:PNP-RIT/my_webapp.git',
                    branch: 'main'
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t \$DOCKERHUB_USER/\$APP_NAME:latest .
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh """
                    echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push \$DOCKERHUB_USER/\$APP_NAME:latest
                """
            }
        }
    }

     post {
        always {
            sh 'echo "This runs always after the pipeline, no steps {} needed"'
        }
        success {
            sh 'echo "Pipeline succeeded!"'
        }
        failure {
            sh 'echo "Pipeline failed!"'
        }
    }

}

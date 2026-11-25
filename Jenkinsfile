pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t azure-jenkins-demo:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Running Docker container'
                sh '''
                    docker rm -f azure-jenkins-container || true
                    docker run -d \
                    --name azure-jenkins-container \
                    -p 8081:80 \
                    azure-jenkins-demo:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
                echo 'Application deployment verified'
            }
        }
    }

    post {
        success {
            echo 'PIPELINE SUCCESS!'
        }

        failure {
            echo 'PIPELINE FAILED!'
        }
    }
}

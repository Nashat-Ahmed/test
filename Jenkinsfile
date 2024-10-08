pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-repo') // Add Docker Hub credentials in Jenkins
        IMAGE_NAME = 'nashaat111/webapp'
        IMAGE_TAG = 'n.1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Nashat-Ahmed/test.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'pytest' // Assuming you have tests defined
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-repo') {
                        dockerImage.push("${IMAGE_TAG}")
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

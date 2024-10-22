pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "nexus:8081"                       // Replace with your Nexus server URL
        DOCKER_REPO = "docker-repo"                          // Replace with your Nexus repository name
        IMAGE_NAME = "back-httpd"                            // Replace with your image name
        NEXUS_CREDENTIALS = credentials('nexusCredentials')  // Nexus credentials ID in Jenkins
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Login to Nexus') {
            steps {
                script {
                    sh "docker login -u ${NEXUS_CREDENTIALS_USR} -p ${NEXUS_CREDENTIALS_PSW} ${DOCKER_REGISTRY}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerImage.push()
                    dockerImage.push('latest') // Optionally push with the latest tag
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_NUMBER} || true"
                    sh "docker rmi ${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:latest || true"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

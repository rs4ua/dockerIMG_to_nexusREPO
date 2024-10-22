pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "nexus:8081"                       // Replace with your Nexus server URL
        DOCKER_REPO = "docker-repo"                          // Replace with your Nexus repository name
        IMAGE_NAME = "back-httpd"                            // Replace with your image name
        NEXUS_CREDENTIALS = credentials('nexusCredentials')  // Nexus credentials ID in Jenkins
        VERSION = "1.1.0"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // List files in the workspace
                    sh 'ls -la'
                
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:${VERSION} ."
                }
            }
        }

        stage('Login to Nexus') {
            steps {
                script {
                    sh "docker login -u ${NEXUS_CREDENTIALS.username} -p ${NEXUS_CREDENTIALS.password} ${DOCKER_REGISTRY}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Define dockerImage variable after building the image
                    def dockerImage = "${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:${VERSION}"
                    
                    // Push the Docker image
                    sh "docker push ${dockerImage}"
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:latest" // Optionally push with the latest tag
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${DOCKER_REGISTRY}/${DOCKER_REPO}/${IMAGE_NAME}:${VERSION} || true"
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

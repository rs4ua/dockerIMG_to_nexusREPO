pipeline {
    agent { docker { image 'python:latest' } }
    environment {
      PROJECT_NAME = "pipeline-test"
      OWNER_NAME   = "Roman Yadzhak"
    }

    stages {
        
        stage('1-Build') {
            steps {
                echo "Start of Stage Build..."
                echo "Building......."
                echo "End of Stage Build..."
            }
        }
        stage('2-Test') {
            steps {
                echo "Start of Stage Test..."
                echo "Testing......."
                echo "Hello ${PROJECT_NAME}"
                echo "Owner is ${OWNER_NAME}"
                echo "End of Stage Build..."
            }
        }
        stage('3-Deploy') {
            steps {
                echo "Start of Stage Deploy..."
                echo "Deploying......."
                sh "ls -la"
                sh '''
                   echo "Line1"
                   echo "Line2"
                '''
                sh "python --version"
                echo "End of Stage Build..."
            }
        }
        stage('4-Celebrate') {
            steps {
                echo "SUCCESS!"
            }
        }	
    }
}

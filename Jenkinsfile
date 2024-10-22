pipeline {
    agent local-agent
    
    stages {
        stage('List Files') {
            steps {
                sh 'ls -la'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t httpd:1.0 .'
            }
        }
        
        stage('Show Docker Images') {
            steps {
                sh 'docker images'
            }
        }
        
        stage('Remove Docker Image') {
            steps {
                sh 'docker rmi httpd:1.0'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

}

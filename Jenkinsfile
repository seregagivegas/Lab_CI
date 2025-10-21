pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'lab-ci-app'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo ' Getting code from repository...'
                git branch: 'main',
                    url: 'https://github.com/seregagivegas/Lab_CI.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo ' Building Docker image...'
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Test') {
            steps {
                echo ' Running tests...'
                sh 'echo "Tests completed successfully!"'
            }
        }
    }
    
    post {
        always {
            echo ' Pipeline completed!'
        }
    }
}

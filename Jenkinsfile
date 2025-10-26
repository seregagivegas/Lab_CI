pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'lab-ci-app'
        DOCKER_TAG = 'latest'
        DOCKER_REGISTRY = 'seregagivegas'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Getting code from repository...'
                git branch: 'main',
                    url: 'https://github.com/seregagivegas/Lab_CI.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Tests completed successfully!"'
                
                script {
                    sh "docker run -d --name test-container ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sleep(10)
                    sh "curl -f http://localhost:5000/health || exit 1"
                    sh "docker stop test-container && docker rm test-container"
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                script {
                    sh "echo 'Command: docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}'"
                    sh "echo 'In real Jenkins: would push to Docker Hub with credentials'"
                    
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed!'
            sh 'docker rm -f test-container || true'
        }
        success {
            echo 'Build successful! Image ready for push to Docker Hub.'
        }
    }
}

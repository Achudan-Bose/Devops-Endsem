pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'your-app-name'
        DOCKER_REGISTRY = 'your-docker-registry'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', url: 'https://your-repository-url.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image for the application
                    sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the image to the Docker registry
                    sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER'
                }
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'staging'
            }
            steps {
                script {
                    // Deploy to the staging environment (Docker)
                    sh 'docker pull $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker run -d -p 8080:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER'
                }
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                script {
                    // Deploy to the production environment (Docker)
                    sh 'docker pull $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker run -d -p 8081:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$BUILD_NUMBER'
                }
            }
        }

        stage('Clean up') {
            steps {
                // Clean up the old Docker containers
                sh 'docker system prune -f'
            }
        }
    }

    post {
        success {
            echo "Deployment successful"
        }
        failure {
            echo "Deployment failed"
        }
    }
}

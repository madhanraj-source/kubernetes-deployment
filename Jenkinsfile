pipeline {
    agent any

    environment {
        // Define environment variables if needed
        DOCKER_IMAGE = 'your-docker-image'
        DOCKER_REGISTRY = 'your-docker-registry'
        DEPLOYMENT_NAMESPACE = 'default'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Pull the latest changes from the repository
                    git 'https://your-repo-url.git'
                    
                    // Build Docker image
                    sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    // Push Docker image to registry
                    sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Apply Kubernetes deployment configurations
                    sh 'kubectl set image deployment/your-deployment your-container=$DOCKER_REGISTRY/$DOCKER_IMAGE:latest --namespace=$DEPLOYMENT_NAMESPACE'
                    sh 'kubectl rollout status deployment/your-deployment --namespace=$DEPLOYMENT_NAMESPACE'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

pipeline {
    agent any

    environment {
        REGISTRY = 'your-docker-registry'
        IMAGE_NAME = 'your-flask-app-image'
        KUBE_CONFIG = credentials('kube-config') // Jenkins credential ID for Kube config
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repository/your-flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://your-docker-registry', 'docker-credentials-id') {
                        def dockerImage = docker.image("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_ID}")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: KUBE_CONFIG]) {
                        sh 'kubectl set image deployment/flask-app-deployment flask-app=${REGISTRY}/${IMAGE_NAME}:${env.BUILD_ID}'
                        sh 'kubectl rollout status deployment/flask-app-deployment'
                    }
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

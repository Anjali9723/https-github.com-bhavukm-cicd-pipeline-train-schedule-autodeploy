pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
        KUBERNETES_CREDENTIALS_ID = 'kubernetes-credentials'
        DOCKER_IMAGE_NAME = 'your-docker-image'
        DOCKER_HUB_REPO = 'your-dockerhub-repo'
        K8S_DEPLOYMENT_NAME = 'your-deployment'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest").push('latest')
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'k8s/deployment.yaml',
                        kubeconfigId: "${KUBERNETES_CREDENTIALS_ID}"
                    )
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment was successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}

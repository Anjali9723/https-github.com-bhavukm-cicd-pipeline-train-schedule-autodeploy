pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'DockerID' 
        KUBERNETES_CREDENTIALS_ID = 'KubernetesID' 
        DOCKER_IMAGE_NAME = 'devops'
        DOCKER_HUB_REPO = 'project'
        K8S_DEPLOYMENT_NAME = 'your-deployment'
    } 

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Anjali9723/https-github.com-bhavukm-cicd-pipeline-train-schedule-autodeploy.git', branch: 'main'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Fixed the image name syntax
                    docker.build("${project}/${devops}:latest")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Use the environment variable for credentials
                    docker.withRegistry('https://index.docker.io/v1/', "${DockerID}") {
                        docker.image("${project}/${devops}:latest").push('latest')
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([string(credentialsId: KubernetesID, variable: 'KUBECONFIG_CONTENT')]) {
                        writeFile(file: 'kubeconfig', text: env.KUBECONFIG_CONTENT)

                        env.KUBECONFIG = 'kubeconfig'

                        sh 'kubectl apply -f deployment.yaml'
                    }
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


        
        
        

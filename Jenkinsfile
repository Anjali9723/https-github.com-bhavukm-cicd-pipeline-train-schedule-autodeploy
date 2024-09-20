pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'DockerID' 
        KUBERNETES_CREDENTIALS_ID = 'KubernetesID' 
        DOCKER_IMAGE_NAME = 'devops2'
        DOCKER_HUB_REPO = 'anjali308'
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
                    
                    docker.build("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest")
                   
                    
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest").push('latest')
                    }
                }
            }
        }
stage('Deploy to Kubernetes') {
    steps {
        script {
            withCredentials([string(credentialsId: 'KUBERNETES_CREDENTIALS_ID', variable: 'KUBECONFIG_CONTENT')]) {
                writeFile(file: 'kubeconfig', text: env.KUBECONFIG_CONTENT)
                env.KUBECONFIG = 'kubeconfig'
                
                // Apply the Kubernetes deployment
                sh 'kubectl apply -f deployment.yaml'

                // Optional: Clean up the kubeconfig file
                sh 'rm -f kubeconfig'
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

       
        
      
    
    
                    
        
       
        
        
        

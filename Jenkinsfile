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
                    // Declare the variable here to make it accessible in the next stage
                    def image = docker.build("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest")
                    // Set it to the environment variable for future access
                    env.DOCKER_IMAGE = image.getImageName()
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Use the image variable for pushing
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image(env.DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([string(credentialsId: KUBERNETES_CREDENTIALS_ID, variable: 'KUBECONFIG_CONTENT')]) {
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

       
        
      
    
    
                    
        
       
        
        
        

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
                    docker.build("${DOCKER_HUB_REPO}/${DOCKER_IMAGE_NAME}:latest")
                }
            }
        }
        stage('Push Docker Image') {
    steps {
        script {
            // Using withCredentials for better handling of sensitive information
            withCredentials([usernamePassword(credentialsId: 'DockerID', usernameVariable: 'anjali308', passwordVariable: 'Gow1110th@m')]) {
                // Log in to Docker
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                
                // Tag the image
                sh 'docker tag project/devops:latest index.docker.io/project/devops:latest'
                
                // Push the image
                sh 'docker push index.docker.io/project/devops:latest'
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

                    
        
       
        
        
        

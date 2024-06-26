pipeline {
    agent any
    
    environment {
        registryCredential = 'ACR' // Credential ID for Azure Container Registry
        dockerImage = 'azjenkinsvmcr.azurecr.io/juice-shop' // Name of your Docker image
        //appName = 'juice-shop' // Azure Web App name
        resourceGroup = 'jenkins-rg' // Azure Resource Group
        dockerfilePath = './Dockerfile' // Path to your Dockerfile in the repo
    }
    
    stages {
        stage('Prepare') {
            steps {
                // Clean workspace
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/adarshlokhandegithub/juice-shop']])
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build Docker image using Dockerfile
                    def dockerImage = docker.build(env.dockerImage, "--file ${env.dockerfilePath} .")
                    
                    // Login to Azure Container Registry
                    docker.withRegistry('https://azjenkinsvmcr.azurecr.io', env.registryCredential) {
                        // Push built Docker image to Azure Container Registry
                        dockerImage.push()
                    }
                }
            }
        }
        
    
    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

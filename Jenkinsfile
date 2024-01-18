pipeline {
    agent any

    environment {
        registryName = "testingimage"
        dockerHubRegistry = "docker.io"
        dockerHubRepo = "samit905787/testingimage"
        webAppName = "dockerhtmlcontainer"
        resourceGroup = "Rg-Amit"
        azureCredentials = 'Azure Service Principal'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    dockerImage = docker.build(registryName)
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_REGISTRY_CREDS', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                        docker.withRegistry("${dockerHubRegistry}", "${dockerHubRepo}", "${dockerHubUser}", "${dockerHubPass}") {
                            dockerImage.push()
                        }
                    }
                }
            }
        }

        stage('Deploy to Azure App Service') {
            steps {
                script {
                    withCredentials([azureServicePrincipal(credentialsId: "${azureCredentials}", tenantId: 'your-tenant-id', clientId: 'your-client-id', secret: 'your-client-secret')]) {
                        sh """
                            az login --service-principal -u \${AZURE_CLIENT_ID} -p \${AZURE_CLIENT_SECRET} --tenant \${AZURE_TENANT_ID}
                            az webapp config container set --name \${webAppName} --resource-group \${resourceGroup} --docker-custom-image-name=${dockerHubRepo}:${env.BUILD_ID}
                        """
                    }
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${registryName}"
                }
            }
        }
    }
}

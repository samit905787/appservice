pipeline {
    agent {
        label 'webserver'
    }  

    environment {
        registryName = "samit905787"
        registryCredential = 'Docker'
        dockerImage = 'testingimage'
        webAppResourceGroup = 'Rg-Amit'
        webAppName = 'dockerhtmlcontainer'   
        registryUrl = 'samit905787'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/samit905787/appservice.git']])
            }
        }

        stage('Build image') {
            steps {        
                script {
                    // Build your Docker image
                    dockerImage = docker.build("${registryName}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push image to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    withDockerRegistry([credentialsId: 'dockerHubCredentials', url: 'https://registry.hub.docker.com']) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Azure App Service') {
            steps {
                script {
                    // Deploy to Azure App Service
                    withCredentials([
                        string(credentialsId: 'app-id', variable: 'username'),
                        string(credentialsId: 'tenant-id', variable: 'tenant'),
                        string(credentialsId: 'app-id-pass', variable: 'password')
                    ]) {
                        sh """
                            az login --service-principal -u ${username} -p ${password} --tenant ${tenant}
                            az webapp config container set --name ${webAppName} --resource-group ${webAppResourceGroup} --docker-custom-image-name=${registryUrl}/${dockerImage}:${env.BUILD_ID}
                        """
                    }
                }
            }
        }
    }
}

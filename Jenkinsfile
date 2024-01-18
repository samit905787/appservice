pipeline {
     agent {
        label 'webserver'
    } 
        environment {
        registryName = "htmlimage"
        registryCredential = 'ACR'
        dockerImage = 'testingimage'
        webAppResourceGroup = 'Rg-Amit'
        webAppName = 'htmlcontainer'   
        registryUrl = 'htmlimage.azurecr.io'
    }
    stages {
        stage ('checkout') {
            steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/samit905787/appservice.git']])
            }
            }
        stage ('build image') {
            steps {        
                script {
                    dockerImage = docker.build("${registryName}:${env.BUILD_ID}")
                     }      
                }
            }
   stage("push"){
            steps{
                withCredentials([usernameColonPassword(credentialsId: 'DOCKER_REGISTRY_CREDS', variable: 'dockerHub'), string(credentialsId: 'dockerhubpass', variable: 'dockerHubPass'), string(credentialsId: 'dockerHubUser', variable: 'dockerHubUser')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag react_django_demo_app:latest ${env.dockerHubUser}/testingimage"
                sh "docker push ${env.dockerHubUser}/testingimage"
                echo 'image push ho gaya'
                }
            }
        }
    
    stage('deploy to appservice') {
        steps {
            withCredentials([
            string(credentialsId: 'app-id', variable: 'username'),
            string(credentialsId: 'tenant-id', variable: 'tenant'),
            string(credentialsId: 'app-id-pass', variable: 'password')
            ]) {
            sh """
                /var/lib/jenkins/.local/bin/az login --service-principal -u ${username} -p ${password} --tenant ${tenant}
                /var/lib/jenkins/.local/bin/az  webapp config container set --name macbookapp --resource-group Rg-Amit  --docker-custom-image-name=htmlimage.azurecr.io/testingimage:${env.BUILD_ID}
                """
            }
        }
    }
    
}
        
}

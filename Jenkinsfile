pipeline {
     agent any
        environment {
        registryName = "bhashini/bhashini"
        registryCredential = 'ACR'
        dockerImage = 'vc'
        registryUrl = 'bhashini.azurecr.io'
        azure-sa = 'jenkins-secrets'
    }
    stages {
        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sonulodha/appservice']]])
            }
            }
        stage ('build image') {
            steps {        
                script {
                    dockerImage = docker.build registryName
                     }      
                }
            }
   // Uploading Docker images into ACR
    stage('Upload Image to ACR') {
     steps{   
         script {
            docker.withRegistry( "http://${registryUrl}", registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }

stage('Deploy Image to ACR') {
     steps{   
        script {
        azureWebAppPublish azureCredentialsId: 'azure-sa', publishType: 'docker',
                   resourceGroup: 'app-service', appName: 'dev-bhashini',
                   dockerImageName: 'bhashini/bhashini', dockerImageTag: '2',
                   dockerRegistryEndpoint: [credentialsId: 'ACR', url: "registryUrl"]

        }
     }
}

    }
}
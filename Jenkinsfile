pipeline {
     agent any
        environment {
        registryName = "bhashini/bhashini"
        registryCredential = 'ACR'
        dockerImage = ''
        registryUrl = 'bhashini.azurecr.io'
    }
    
    stages {
        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'github.com/sonulodha/appservice']]])
            }
        }
       
   stage ('Build image') {
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
      
    stage('Docker ps') {
     steps{
         script {
                sh 'docker ps'
            }
      }
    }
    }
 }
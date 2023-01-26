pipeline {
     agent any
        environment {
        registryName = "bhashini/bhashini"
        registryCredential = 'ACR'
        dockerImage = 'vc'
        registryUrl = 'bhashini.azurecr.io'
    }
    stages {
        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sonulodha/appservice']]])
            }
            }
        stage('stop previous containers') {
            steps {
            sh 'pwd'
            }
            }
        stage ('build image') {
            steps {        
                script {
                    dockerImage = docker.build registryName
                     }      
                }
            }
      
        }
 }
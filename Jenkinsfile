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
        stage('build docker images') {
            steps {
            sh 'id'
            sh '/opt/homebrew/bin/docker buildx build --platform linux/amd64 -t bhashini.azurecr.io/bhashini:v1 '
            }
            }
      
        }
 }
pipeline {
  environment {
    registry = "harshu004/docker-nodejs"
    registryCredential = dockerhub
    dockerImage = ''
  }
  agent any
  
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
  }
}


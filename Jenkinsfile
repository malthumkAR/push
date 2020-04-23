pipeline {
  environment {
    registry = "vinod9782/inspired"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }    
  agent any
  tools {maven "maven" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/hrmanth/game-of-life.git'
      }
    }
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
         }
       }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }    
          }
        }
      }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker run -d --name Inspired4 -p 8089:8080 vinod9782/inspired:$BUILD_NUMBER"
      }
    }     
      }
   }


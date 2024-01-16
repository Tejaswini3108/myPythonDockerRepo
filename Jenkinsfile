pipeline {
    agent any
    environment {
        registry = "439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Tejaswini3108/myPythonDockerRepo.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 439042917544.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo:latest'
         }
        }
      }
   
         
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8082:5000 --rm --name mypythonContainer 439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo:latest'
            }
      }
    }
  }
}

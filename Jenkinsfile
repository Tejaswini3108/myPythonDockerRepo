pipeline{
    agent any
    environment{
        registry = "439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo"
    }
    stages{
        stage("checkout"){
            steps{
            checkout checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url:[[url: 'https://github.com/Tejaswini3108/myPythonDockerRepo.git']])
            }
        }
        stage("Docker Build"){
            steps{
                script{
                     dockerImage = docker.build registry
                }

            }
        }

        stage("Docker Push"){
            steps{
                script{
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 439042917544.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo:latest'
                }
            }
            stage('Docker Run'){
                steps{
                    script{
                        sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 439042917544.dkr.ecr.us-east-1.amazonaws.com/test2-repo:latest'
                    }
                }
            }
        }
    }
}

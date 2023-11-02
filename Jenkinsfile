pipeline {
    environment {
    IMAGE_REPO_NAME = "sharukdoc/kasplo-api"
    IMAGE_TAG = "latest"
  }
  agent any

  stages {
    stage('Cloning Git') {
       steps {
         checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/sharu1301/kasplo-api.git/']]])     
            }
    }
        
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'docker-creds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yml", kubeconfigId: "kubernetes")
          kubernetesDeploy(configs: "service.yml", kubeconfigId: "kubernetes")
        }
      }
    }
  }
  

}

pipeline {

  environment {
    dockerimagename = "rithu2008/testapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Rithu2008/node-app.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'DockerHubLogin'
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
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "KubeConfig")
        }
      }
    }

  }

}

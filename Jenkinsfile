pipeline {

  environment {
    dockerimagename = "indalarajesh/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/INDALARAJESH/nodeapp_test.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = 'docker build -t indalarajesh/nodeapp .'
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
          sh docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
          sh dockerImage.push("latest")
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "minikube")
        }
      }
    }

  }

}

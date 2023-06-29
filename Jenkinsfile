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

    stage('Docker Build & Push') {
    steps {
        script {
            withDockerRegistry(credentialsId: 'dockerhublogin', toolName: 'docker') {
                sh "docker build -t nodeapp:latest -f docker/Dockerfile ."
                sh "docker tag nodeapp:latest indalarajesh/nodeapp:latest"
                sh "docker push indalarajesh/nodeapp:latest"
            }
        }
    }
}



    // stage('Build image') {
    //   steps{
    //     script {
    //       dockerImage = 'docker build -t indalarajesh/nodeapp .'
    //     }
    //   }
    // }

    // stage('Pushing Image') {
    //   environment {
    //            registryCredential = 'dockerhublogin'
    //        }
    //   steps{
    //     script{
    //       docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
    //       dockerImage.push("latest")
    //       }
    //     }
    //   }
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

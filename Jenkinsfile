pipeline {

  environment {
    dockerimagename = "n0face/git-apache"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/n0f4ce/jenkins-github-repo.git', credentialsId: 'github'
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
               registryCredential = 'docker-hub'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying git-apache container to kubernetes') {
      steps {
        script {
          bat 'kubectl apply -f deployment.yaml -f service.yaml'
        }
      }
    }

  }

}

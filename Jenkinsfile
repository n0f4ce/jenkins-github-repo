pipeline {
    environment {
        dockerimagename = "n0face/git-apache"
        dockerImage = ""
        dockerTool = 'docker'  // Docker tool name
        registryCredential = 'docker-hub'
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/n0f4ce/jenkins-github-repo.git', credentialsId: 'github'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.withTool(dockerTool).build(dockerimagename)
                }
            }
        }

        stage('Pushing Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying git-apache container to kubernetes') {
            steps {
                script {
                    sh 'kubectl config get-contexts'
                    sh 'kubectl get pods -n automate-deploy'
                }
            }
        }
    }
}

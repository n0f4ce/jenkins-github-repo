pipeline {
    agent {
        node 'jenkins-agent-2'
    }
    environment {
        dockerimagename = "n0face/git-apache"
        dockerImage = ""
    }


    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/n0f4ce/jenkins-github-repo.git', credentialsId: 'github'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'docker-hub'
            }
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
                    sh 'kubectl apply -f deployment.yaml -n project-namespace'
                    sh 'kubectl apply -f automate-deploy -n project-namespace'
                }
            }
        }
    }
}

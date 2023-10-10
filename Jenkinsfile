pipeline {
    agent {
        node 'jenkins-agent-1'
    }

    /*environment {
        dockerImageName = "n0face/git-apache"
        registryCredential = 'docker-hub'
    }*/

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/n0f4ce/jenkins-github-repo.git', credentialsId: 'github'
            }
        }
        //docker commands
        /*stage('Build image') {
            steps {
                script {
                    // Build the Docker image
                    def customImage = docker.image(dockerImageName).build()
                }
            }
        }

        stage('Pushing Image') {
            steps {
                script {
                    // Push the Docker image to a registry
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        customImage.push("latest")
                    }
                }
            }
        }*/

        stage('Deploying git-apache container to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment configuration
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}

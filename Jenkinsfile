pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'alexb00/devops-demo-app'
    }

    stages {
        stage('Clone repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Krashmazy/DevOps10.git'
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_HUB_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: 'Kubernetes/deployment.yaml')
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

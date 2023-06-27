pipeline {
    agent any
    environment {
        dockerImageName = "naresh1985/node-hello-world"
        dockerImageTag = "1.1"
        dockerImage = ""
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/Naresh-1954/docker-node-hello-world.git']]])
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build("${dockerImageName}:${dockerImageTag}")
                }
            }
        }

        stage('Tag image') {
            steps {
                script {
                    def image = docker.image("${dockerImageName}:${dockerImageTag}")
                    image.tag("${dockerImageName}:latest")
                    sh "docker tag ${dockerImageName}:${dockerImageTag} ${dockerImageName}:custom-tag"
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerlogin'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("${dockerImageName}:${dockerImageTag}")
                        dockerImage.push("${dockerImageName}:latest")
                        dockerImage.push("${dockerImageName}:custom-tag")
                    }
                }
            }
        }
    }
}

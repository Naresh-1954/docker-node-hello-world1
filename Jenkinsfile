pipeline {
    agent any
    environment {
        dockerImageName = "naresh1985/node-hello-world"
        dockerImageTag = "1.1"
        dockerRegistry = "registry.hub.docker.com"
        dockerRegistryCredentials = "dockerlogin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/Naresh-1954/docker-node-hello-world.git']]])
            }
        }

        stage('Build and Push Image') {
            steps {
                script {
                    def imageTag = "${dockerImageName}:${dockerImageTag}"
                    docker.build(imageTag)
                    docker.withRegistry(dockerRegistry, dockerRegistryCredentials) {
                        docker.image(imageTag).push()
                    }
                }
            }
        }
    }
}

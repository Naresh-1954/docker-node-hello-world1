pipeline {
    agent any
    environment {
        dockerImageName = "naresh1985/node-hello-world"
        dockerImageTag = "${BUILD_NUMBER}"
        dockerRegistry = "https://registry.hub.docker.com"
        dockerRegistryCredentials = "dockerlogin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Naresh-1954/docker-node-hello-world1.git']]])
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

        stage('Trigger the config change in pipeline') {
            steps {
                script {
                    sh "curl -v -k -user admin:118f4e302d009de38e65d67c61f94a26fa -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' -d 'IMAGE_TAG=${dockerImageTag}' 'http://3.83.105.215:8080/job/democd/buildWithParameters?token=gitops-config'"
                }
            }
        }
    }
}

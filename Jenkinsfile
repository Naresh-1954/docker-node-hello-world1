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
                    sh "curl -v -k -user admin:116c74bba9d03fd70e2fa8b6188663aaaa -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' -d 'IMAGE_TAG=${dockerImageTag}' 'http://50.19.171.233:8080/job/gitops-argocd_CD/buildWithParameters?token=gitops-config'"
                }
            }
        }
    }
}

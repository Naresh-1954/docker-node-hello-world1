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
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/nani1231234/docker-node-hello-world1.git']]])
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

        stage('Updating Kubernetes Deployment File') {
            steps {
                script {
                    sh """
                    cat deployment.yaml
                    sed -i 's@${dockerImageName}.*@${dockerImageName}:${dockerImageTag}@g' deployment.yaml
                    cat deployment.yaml
                    """
                }
            }
        }
        
		stage('Push the changed deployment file to Git') {
          steps {
				script {
					sh """
						
      					git config --global user.name "Naresh-1954"
						git config --global user.mail "nareshcse31@gmail.com"
						git fetch origin
                        git pull origin main
                        git branch -m master main
						git add deployment.yaml
						git commit --amend --reset-author -m "updated the deployment file"
						git push https://github.com/nani1231234/docker-node-hello-world1.git main
					"""
				}
            }
        }


    }
}

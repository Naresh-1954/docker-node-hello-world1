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
        
        stage('Push Deployment File to GitHub') {
            steps {
                script {
                    sh """
		    git pull  https://github.com/Naresh-1954/docker-node-hello-world1.git main
                    git config --global user.name "Naresh-1954"
                    git config --global user.email "nareshcse31@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update deployment file"
		     git checkout main
		    git push origin main
	         	"""
		    withCredentials([gitUsernamePassword(credentialsId: 'github-user', gitToolName: 'Default')]) {
                        sh """
				        git checkout main
                  			git pull origin main
                    			git push origin main
                           """
                    }

                }
            }
        }
    }
}


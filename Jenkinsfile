pipeline {
  agent any
  tools {
    maven 'maven-3.6.3'
  }
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Docker Build') {
      steps {
        script {
                docker.build("ddyadav/hello-world:${env.BUILD_ID}")
        }
      }
    }
	stage ('Docker Build') {
      steps {
        script {
				withDockerRegistry(credentialsId: 'dockercredentials', url: 'https://registry.hub.docker.com/') {
				   docker.image("ddyadav/hello-world:${env.BUILD_ID}").push()
				   docker.image("ddyadav/hello-world:${env.BUILD_ID}").push("latest")
				   
				}
		}
      }
    }
    stage ('Deploy') {
      steps {
		sh 'docker stop hello-world | true'
		sh 'docker rm hello-world | true'
		sh 'docker run -d --name hello-world -p 8089:80 ddyadav/hello-world:${env.BUILD_ID}'
      }
    }	
  }
}

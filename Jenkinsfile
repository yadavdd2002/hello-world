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
    stage ('Deploy') {
      steps {
		sh "docker stop hello-world | true"
		sh "docker rm hello-world | true"
		sh "docker run -d --name hello-world -p 8089:80 ddyadav/hello-world:${env.BUILD_ID}"
      }
    }
  }
}


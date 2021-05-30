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
    stage('SonarQube analysis') {
        environment {
          scannerHome = tool 'sonarqube_scanner'
        }
        steps {
              withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    stage ('Docker Build') {
      steps {
        script {
                docker.build("ddyadav/hello-world:${env.BUILD_ID}")
        }
      }
    }

	stage('Pushing Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockercredentials') {
                        docker.image("ddyadav/hello-world:${env.BUILD_ID}").push()
                        docker.image("ddyadav/hello-world:${env.BUILD_ID}").push("latest")
                    }
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

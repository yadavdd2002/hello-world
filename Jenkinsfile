pipeline {
  agent any
  tools {
    maven 'maven-3.6.3'
  }
  environment {
      DATE = new Date().format('yy.M')
      TAG = "${DATE}.${BUILD_NUMBER}"
      scannerHome = tool 'sonarqube_scanner'
  }
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('SonarQube analysis') {
        steps {
              withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    stage ('Docker Build') {
      steps {
        script {
                docker.build("ddyadav/hello-world:${TAG}")
        }
      }
    }

	stage('Pushing Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockercredentials') {
                        docker.image("ddyadav/hello-world:${TAG}").push()
                        docker.image("ddyadav/hello-world:${TAG}").push("latest")
                    }
                }
            }
        }	

  stage('Anchore Scanning') {
            steps {
                script {
                    def imageLine = "ddyadav/hello-world:${TAG}"
                    writeFile file: 'anchore_images', text: imageLine
                    anchore name: 'anchore_images', bailOnFail: false
                }
            }
        }    
  stage ('Deploy') {
      steps {
		sh "docker stop hello-world | true"
		sh "docker rm hello-world | true"
		sh "docker run -d --name hello-world -p 8089:80 ddyadav/hello-world:${TAG}"
      }
    }	
  }
}

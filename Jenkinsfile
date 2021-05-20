pipeline {
    agent any
    tools {
        maven '3.8.1'
    }
    stages {
        stage ('Build War File') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ('Deploy War File') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat_credential', path: '', url: 'http://localhost:8081')], contextPath: '/pipeline', onFailure: false, war: 'webapp/target/*.war'
            }
        }
    }
}

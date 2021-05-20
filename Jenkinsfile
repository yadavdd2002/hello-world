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
    }
}

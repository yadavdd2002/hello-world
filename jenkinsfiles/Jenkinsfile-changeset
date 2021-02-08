pipeline {
    agent any
    tools {
        maven 'maven-3.6.3' 
    }
    stages {
        stage ('Check maven version') {
            steps {
                sh 'mvn --version'
            }
        }
        stage ('Generate package for app1') {
            when { changeset "app1/*"}
            steps {
                dir ('app1') {
                    sh 'mvn clean package'
                }
            }
        }
        stage ('Generate package for app2') {
            when { changeset "app2/*"}
            steps {
                dir ('app2') {
                    sh 'mvn clean package'
                }
            }
        }
        stage ('Generate package for app3') {
            when { changeset "app3/*"}
            steps {
                dir ('app3') {
                    sh 'mvn clean package'
                }
            }
        }
    }
}

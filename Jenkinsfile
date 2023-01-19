pipeline {
    agent any
    tools { 
        maven 'm3' 
    }
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Docker') {
            steps {
                sh 'docker ps'
            }
        }
    }
}



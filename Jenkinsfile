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
        stage('Test-sonar'){
        when {
                branch 'main'
            }
            steps {
                sh 'make check'
                junit 'reports/**/*.xml' 
            }
        }
       stage('Deploy') {
        when {
                branch 'main' 
            }
            steps {
                sh 'echo publish'
                sh 'kubeclt apply -f ingress.yaml'
            }
        } 

    }
}



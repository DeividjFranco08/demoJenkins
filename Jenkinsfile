pipeline {
    agent any
    tools { 
        maven 'm3' 
    }
    stages {
        stage ('Build') {
            when {
                branch 'main' 
            }
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
    }
}



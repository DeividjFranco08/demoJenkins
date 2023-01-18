def ARTIFACTORY_DOCKER_REGISTRY="gcr.io/arquitectura-digital-everis/"

pipeline {
  agent {
    kubernetes {
      label 'demo-scotiabank'
      defaultContainer 'jnlp'
      yamlFile 'devops/agent.yml'
    }
  }
  tools {
          gradle "7.4.2"
      }

  stages {
    stage('Build and unit tests') {
      when {
        anyOf {
          branch 'feature/*'
        }
      }
      steps {
          script {
            sh 'ls -l ./gradlew'
            sh 'whoami'
            sh 'chmod +x -R ./gradlew'
            sh './gradlew build'
          }
      }
    }

    stage('SonarQube Analysis') {
      when {
        anyOf {
          branch 'feature/*'
        }
      }

      steps {
        withSonarQubeEnv('sonarqube') {
         sh "whoami"
         sh "./gradlew sonarqube"
         }
      }
    }


    stage ('Build docker image') {
      when {
        anyOf {
          branch 'feature/*'
        }
      }
      steps {
        container ('docker-agent'){
          script {
            app = docker.build(ARTIFACTORY_DOCKER_REGISTRY + 'demo-scotia:latest', ' .')
          }
        }
      }
    }

    stage ('Push image to Artifactory') {
      when {
        anyOf {
          branch 'feature/*'
        }
      }
      steps {
        container ('docker-agent'){
          script {
            docker.withRegistry('https://gcr.io', 'gcr:arquitectura-digital-everis') {
              app.push()

            }
          }

        }
      }
    }
    /**
    stage('upload artifactory'){

      when {
        anyOf {
          branch 'feature/*'
        }
      }

      steps {
        container ('docker-agent'){
          script {
            withCredentials([file(credentialsId: 'sa-demo-scotia', variable: 'GC_KEY')]) {
              sh 'gcloud auth activate-service-account --key-file=${GC_KEY}'
              sh 'gcloud auth list'
              sh 'gcloud projects list'
              sh 'gcloud config set project arquitectura-digital-everis'
              sh 'gcloud projects list'
              sh 'gcloud alpha artifacts repositories list'
              sh './gradlew publish'
            }
          }
        }
      }

    }
    **/
  }

}



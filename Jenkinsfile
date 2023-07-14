pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }

  stages {

    stage('Find directory'){
        steps{
            sh 'pwd'
            sh 'ls -la'
        }
    }

    stage('Create Jar') {
      steps {
            sh 'docker compose -f docker-compose-jar.yml up'
      }
    }

    stage('Build') {
      steps {
       sh 'docker compose -f docker-compose-build-prod.yml build'
      }
    }

    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Push') {
      steps {
         sh 'docker compose -f docker-compose-build-prod.yml push'
      }
    }
  }

  post {
    always {
      sh 'docker logout'
     }
    }
 }
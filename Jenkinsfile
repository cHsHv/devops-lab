pipeline {
      agent any
      environment {
          LAB_NAME = 'devops-lab'
      }   
      stages {
          stage('Hello') {
              steps {
                  echo "Starting ${env.LAB_NAME}, build #${env.BUILD_NUMBER}"
              }
          }  
          stage('Inspect') {
              steps {
                 sh 'cat /etc/os-release'
              }   
          }
          stage('AWS Identity') {
              steps {
                  withCredentials([usernamePassword(
                      credentialsId: 'aws-lab-jenkins',
                      usernameVariable: 'AWS_ACCESS_KEY_ID',
                      passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                  )]) {
              sh 'aws sts get-caller-identity'
                  }
              }
          }
          stage('AWS credentials') {
              steps {
                 sh 'echo "leaked: $AWS_SECRET_ACCESS_KEY"'
              }
          }
      }   
      post {
          always  { echo 'Build finished.' }
          success { echo 'Green.' }
          failure { echo 'Red.' } 
      }   
  }  

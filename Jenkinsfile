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
          stage('write file') {
              steps {
                 sh "echo ${env.BUILD_NUMBER} > build_number.txt"
                 sh 'cat build_number.txt'
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
              sh 'echo "leaked: $AWS_SECRET_ACCESS_KEY"'
                  }
              }
          }
      }   
      post {
          always  { echo 'Build finished.' }
          success { echo 'Green.' }
          failure { echo 'Red.' } 
      }   
  }  

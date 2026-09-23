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
                 sh 'pwd'
                 sh 'id'
              }   
          }
      }   
      post {
          always  { echo 'Build finished.' }
          success { echo 'Green.' }
          failure { echo 'Red.' } 
      }   
  }  

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
                  sh "aws s3 cp build_number.txt s3://na-pesho-kofata/build/${env.BUILD_NUMBER}/build_number.txt"
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

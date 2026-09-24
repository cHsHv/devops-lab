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
                  sh "aws s3 ls s3://na-pesho-kofata/build/${env.BUILD_NUMBER}/"
                  sh "aws s3 cp s3://na-pesho-kofata/build/${env.BUILD_NUMBER}/build_number.txt ."
                  sh 'cat build_number.txt'
                  sh "aws s3 rm s3://na-pesho-kofata/build/ --recursive"
                  sh '''
                     if out=$(aws iam list-users 2>&1); then
                       echo "FAIL: iam:ListUsers SUCCEEDED — AdministratorAccess is still attached"
                       echo "$out" 
                       exit 1
                     fi 
                       echo "$out" | grep -q AccessDenied || {
                       echo "FAIL: command failed, but not with AccessDenied. Actual output:"
                       echo "$out"
                       exit 1
                     }
                     echo "OK: iam:ListUsers denied — AccessDenied confirmed"
                     '''
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

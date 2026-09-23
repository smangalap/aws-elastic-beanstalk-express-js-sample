pipeline {
   
    agent any

    environment {
       
        CI = 'true'
        IMAGE_NAME = 'smangalap/isec6000-assessment-2'
    }

    stages {

        stage('Install Dependencies') {

            agent {
                docker {
                    image 'node:16'
                    args '-u root'
                    reuseNode true
                }
            }

            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
            }
        }

        stage('Test') {

            agent {
                docker {
                    image 'node:16'
                    args '-u root'
                    reuseNode true
                }
            }

            steps {
                sh 'npm test'
            }
        }

        stage('Security Scan') {

            agent {
                docker {
                    image 'node:16'
                    args '-u root'
                    reuseNode true
                }
            }

            steps {
                echo 'Scanning dependencies for High and Critical vulnerabilities...'
                sh 'npm audit --audit-level=high'
            }
        }

        stage('Build Docker Image') {
        
            steps {
                sh 'docker version'

                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

       stage('Push Docker Image') {
           steps {
               withCredentials([usernamePassword(
                   credentialsId: 'dockerhub-credentials',
                   usernameVariable: 'DOCKER_USER',
                   passwordVariable: 'DOCKER_TOKEN'
               )]) {
                   sh 'echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin'
                   sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
               }
          }
 
      }
    }
    
    post {

        always {
            echo "Pipeline finished with result: ${currentBuild.currentResult}"
        }

        success {
            echo 'Install, test, security scan, Docker build, and Docker push stages passed.'
        }

        failure {
            echo 'Pipeline failed. Check the stage logs above for details.'
        }
    }
}

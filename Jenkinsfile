pipeline {

    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }

    environment {
        CI = 'true'
        HOME = '.'
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

	stage('Security Scan') {
    	    steps {
                echo 'Scanning dependencies for High and Critical vulnerabilities...'
                sh 'npm audit --audit-level=high'
            }
        }

    }

    post {
        always {
            echo "Pipeline finished with result: ${currentBuild.currentResult}"
        }

        success {
            echo 'Install, test and security scan stages passed.'
        }

        failure {
            echo 'Pipeline failed. Check the logs above.'
        }
    }
}

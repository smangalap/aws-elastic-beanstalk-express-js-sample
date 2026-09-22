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
    }

    post {
        always {
            echo "Pipeline finished with result: ${currentBuild.currentResult}"
        }

        success {
            echo 'Install and test stages passed.'
        }

        failure {
            echo 'Pipeline failed. Check the logs above.'
        }
    }
}

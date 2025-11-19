pipeline {
    agent any

    environment {
        // Update this path to YOUR installed Node.js path
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checkout code'
                checkout scm
                bat 'git --version'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application'
                bat 'node -v'
                bat 'npm -v'
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('Parallel Execution') {
            parallel {
                
                stage('Unit Tests') {
                    steps {
                        echo 'Running Unit Tests'
                        bat 'npm run test:unit'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        echo 'Running Integration Tests'
                        bat 'npm run test:integration'
                    }
                }

                stage('Code Quality Check') {
                    steps {
                        echo 'Running Code Quality Checks'
                        bat 'npm run quality'
                    }
                }

            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'All parallel stages passed → Deploying...'
                bat 'echo Deployment steps here'
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Status: ${currentBuild.result}"
        }
        success {
            echo "Pipeline SUCCESS"
        }
        failure {
            echo "Pipeline FAILED"
        }
    }
}

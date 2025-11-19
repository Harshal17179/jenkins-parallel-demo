pipeline {
    agent any

    tools {
        nodejs "Node18"
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
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('Parallel Execution') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        bat 'npm run test:unit'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        bat 'npm run test:integration'
                    }
                }
                stage('Code Quality Check') {
                    steps {
                        bat 'npm run quality'
                    }
                }
            }
        }
    }
}

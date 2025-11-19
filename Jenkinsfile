pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Stage: Checking out code from Git repository'
                checkout scm
                sh 'git --version'
                echo 'Repository cloned successfully'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Stage: Building the application'
                sh 'npm install'
                sh 'npm run build'
                echo 'Build completed successfully'
            }
        }
        
        stage('Parallel Execution') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'Parallel Branch: Running Unit Tests'
                        sh 'npm run test:unit'
                        echo 'Unit tests completed'
                    }
                    post {
                        always {
                            echo 'Unit Tests stage completed'
                            // Archive test results if needed
                            archiveArtifacts artifacts: 'coverage/**/*', allowEmptyArchive: true
                        }
                    }
                }
                
                stage('Integration Tests') {
                    steps {
                        echo 'Parallel Branch: Running Integration Tests'
                        sh 'npm run test:integration'
                        echo 'Integration tests completed'
                    }
                    post {
                        always {
                            echo 'Integration Tests stage completed'
                        }
                    }
                }
                
                stage('Code Quality Check') {
                    steps {
                        echo 'Parallel Branch: Running Code Quality Checks'
                        sh 'npm run quality'
                        echo 'Code quality check completed'
                    }
                    post {
                        always {
                            echo 'Code Quality Check stage completed'
                        }
                    }
                }
            }
        }
        
        stage('Deploy') {
            when {
                allOf {
                    expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
                }
            }
            steps {
                echo 'Stage: Deploying application (runs only if all parallel stages succeed)'
                sh 'echo "Deployment would happen here"'
                sh 'echo "All parallel stages completed successfully - proceeding with deployment"'
                archiveArtifacts artifacts: '**/*', excludes: 'node_modules/**,coverage/**,.git/**'
            }
        }
    }
    
    post {
        always {
            echo '=========================================='
            echo 'Pipeline execution completed'
            echo "Build Status: ${currentBuild.result ?: 'SUCCESS'}"
            echo "Build Number: ${env.BUILD_NUMBER}"
            echo "Build Duration: ${currentBuild.durationString}"
            echo '=========================================='
            // Clean workspace if needed
            // cleanWs()
        }
        success {
            echo 'Pipeline executed successfully!'
            echo 'All parallel stages completed successfully'
        }
        failure {
            echo 'Pipeline failed!'
            echo 'One or more stages failed'
            // Send notifications, etc.
        }
        unstable {
            echo 'Pipeline is unstable'
            echo 'Some tests may have failed'
        }
    }
}


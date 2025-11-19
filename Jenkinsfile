pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Stage: Checking out code from Git repository'
                checkout scm
                bat 'git --version'
                echo 'Repository cloned successfully'
            }
        }

        stage('Build') {
            steps {
                echo 'Stage: Building the application'
                bat 'npm install'
                bat 'npm run build'
                echo 'Build completed successfully'
            }
        }
        
        stage('Parallel Execution') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'Parallel Branch: Running Unit Tests'
                        bat 'npm run test:unit'
                        echo 'Unit tests completed'
                    }
                    post {
                        always {
                            echo 'Unit Tests stage completed'
                            archiveArtifacts artifacts: 'coverage/**/*', allowEmptyArchive: true
                        }
                    }
                }
                
                stage('Integration Tests') {
                    steps {
                        echo 'Parallel Branch: Running Integration Tests'
                        bat 'npm run test:integration'
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
                        bat 'npm run quality'
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
                bat 'echo "Deployment would happen here"'
                bat 'echo "All parallel stages completed successfully - proceeding with deployment"'
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
        }
        success {
            echo 'Pipeline executed successfully!'
            echo 'All parallel stages completed successfully'
        }
        failure {
            echo 'Pipeline failed!'
            echo 'One or more stages failed'
        }
        unstable {
            echo 'Pipeline is unstable'
            echo 'Some tests may have failed'
        }
    }
}

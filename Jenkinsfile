pipeline {
    agent any

    stages {

        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'echo Running Unit Tests'
                        sh 'sleep 5'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh 'echo Running Integration Tests'
                        sh 'sleep 7'
                    }
                }

                stage('Code Quality Check') {
                    steps {
                        sh 'echo Running Code Quality Check'
                        sh 'sleep 4'
                    }
                }
            }
        }

        stage('After All Success') {
            when {
                succeeded()
            }
            steps {
                echo "All tests passed successfully!"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed (always)."
        }
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}

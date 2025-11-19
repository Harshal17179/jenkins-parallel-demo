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
            steps {
                script {
                    echo "This stage runs only if all parallel stages are successful."
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished (always)."
        }
        success {
            echo "Pipeline success!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}

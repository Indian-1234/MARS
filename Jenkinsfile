pipeline {
    agent any

    stages {
        stage('Environment Setup') {
            steps {
                script {
                    // Check Node.js version
                    def nodeVersion = sh(script: 'node --version', returnStdout: true).trim()
                    echo "Node.js version: ${nodeVersion}"

                    // Check npm version
                    def npmVersion = sh(script: 'npm --version', returnStdout: true).trim()
                    echo "npm version: ${npmVersion}"
                }
            }
        }

        stage('NPM Version Check') {
            steps {
                script {
                    // Run npm version check
                    sh 'npm --version'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

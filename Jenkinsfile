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

        stage('NPM Install in Client') {
            steps {
                dir('client') {
                    // Change to your client directory path
                    script {
                        // Install npm packages
                        sh 'npm install'
                    }
                }
            }
        }

        stage('NPM Version Check') {
            steps {
                script {
                    // Verify npm version after installation
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

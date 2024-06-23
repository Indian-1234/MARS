pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your Git repository
                checkout scm
            }
        }
        
        stage('Environment Setup') {
            steps {
                // Set up environment (Node.js and npm versions check)
                script {
                    def nodeVersion = sh(script: 'node --version', returnStdout: true).trim()
                    def npmVersion = sh(script: 'npm --version', returnStdout: true).trim()
                    echo "Node.js version: ${nodeVersion}"
                    echo "npm version: ${npmVersion}"
                }
            }
        }
        
        stage('NPM Install') {
            steps {
                // Install npm dependencies
                dir('client') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Build') {
            steps {
                // Run build commands here (in the 'client' directory)
                dir('client') {
                    sh 'npm run build'
                }
            }
        }
        
        stage('Archive Build Artifacts') {
            steps {
                // Archive the build artifacts (e.g., 'build' directory)
                dir('client') {
                    archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                }
            }
        }
        
        stage('Post Actions') {
            steps {
                echo 'Pipeline completed successfully!'
            }
        }
    }
    
    post {
        always {
            echo 'This will always run'
        }
    }
}

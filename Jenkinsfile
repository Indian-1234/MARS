pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository, specifying the 'main' branch
                git branch: 'main', url: 'https://github.com/Indian-1234/MARS.git'
            }
        }
        
        stage('Build') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                dir('client') {
                    sh 'npm install'
                    sh 'npm run build'
                    
                    // Archive build artifacts (example: assuming 'dist' directory)
                    archiveArtifacts artifacts: 'dist/**'
                }
            }
        }
        
        stage('Test') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                dir('client') {
                    sh 'npm test'
                }
            }
        }
        
        stage('Deploy') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                dir('client') {
                    sh 'npm run deploy'
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

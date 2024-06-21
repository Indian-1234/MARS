pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git 'https://Indian-1234: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@github.com/Indian-1234/MARS.git'
            }
        }
        
        stage('Build') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                sh 'npm install'
                sh 'npm run build'
                
                // Archive build artifacts (example: assuming 'dist' directory~
                archiveArtifacts artifacts: 'dist/**'
            }
        }
        
        stage('Test') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            when {
                // Define a condition to trigger only for 'main' branch
                branch 'main'
            }
            steps {
                sh 'npm run deploy'
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

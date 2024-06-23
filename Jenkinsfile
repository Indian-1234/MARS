pipeline {
    agent any

    environment {
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git branch: "${GIT_BRANCH}", url: 'https://github.com/Indian-1234/MARS.git', credentialsId: '1d3f1a5e-fec2-43dd-95c7-004989bef576'
                echo "Branch name: ${env.BRANCH_NAME}"
            }
        }
        
        stage('Build') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
            }
            steps {
                dir('client') {
                    sh 'npm install'
                    sh 'npm run build'
                    
                    // Archive build artifacts
                    archiveArtifacts artifacts: 'dist/**'
                }
            }
        }
        
        stage('Test') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
            }
            steps {
                dir('client') {
                    sh 'npm test'
                }
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main'
                }
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

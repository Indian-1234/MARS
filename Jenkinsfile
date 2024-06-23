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
            steps {
                dir('client') {
                    // Assuming this is a Node.js project
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Echo After Success') {
            steps {
                echo 'Pipeline completed successfully!'
            }
        }
    }

    post {
        success {
            echo 'All stages completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

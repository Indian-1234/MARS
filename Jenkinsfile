pipeline {
    agent any

    environment {
        GIT_BRANCH = 'main'
        NODEJS_HOME = tool name: 'NodeJS 14.17.0', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
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
                    // Use NODEJS_HOME environment variable to set NodeJS path
                    sh "${NODEJS_HOME}/bin/npm install"
                    sh "${NODEJS_HOME}/bin/npm run build"
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

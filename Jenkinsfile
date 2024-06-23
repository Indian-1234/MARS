pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'mars-deploy' // Replace with your actual Jenkins credentials ID
        VPS_HOST = '185.199.53.88'         // Replace with your VPS IP or hostname
        VPS_USER = 'root'                  // Replace with your VPS username
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                checkout scm
            }
        }

        stage('Environment Setup') {
            steps {
                echo 'Setting up the environment...'
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
                echo 'Installing npm dependencies...'
                dir('client') {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                dir('client') {
                    withEnv(['CI=false']) {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                echo 'Archiving build artifacts...'
                dir('client') {
                    archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add your actual test steps here if needed
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sshagent(credentials: [env.VPS_CREDENTIALS]) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${env.VPS_USER}@${env.VPS_HOST} '
                                cd /home/marsinstitute/htdocs/www.marsinstitute.in/MARS &&
                                source ~/.nvm/nvm.sh &&
                                nvm install 18.17.0 &&
                                nvm use 18.17.0 &&
                                echo \$PATH &&
                                npm --version &&
                                npm start > app.log 2>&1 &
                            '
                        """
                        // Wait for the application to start (adjust timeout if needed)
                        sh "sleep 30"
                    }
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
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'mars-deploy' // Jenkins credentials ID for SSH
        VPS_HOST = '185.199.53.88'       // IP or hostname of your VPS
        VPS_USER = 'root'                // SSH username for your VPS
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                // Add your actual checkout steps here if needed
                // Example: git checkout ...
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add your actual build steps here if needed
                // Example: mvn clean install
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add your actual test steps here if needed
                // Example: mvn test
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'

                // Use SSH agent to securely connect to the VPS
                sshagent(credentials: [env.VPS_CREDENTIALS]) {
                    script {
                        // SSH into the VPS and perform deployment steps
                        sh """
                            ssh -o StrictHostKeyChecking=no ${env.VPS_USER}@${env.VPS_HOST} '
                                cd /home/marsinstitute/htdocs/www.marsinstitute.in/backend &&
                                source ~/.nvm/nvm.sh &&
                                nvm install 18.17.0 &&
                                nvm use 18.17.0 &&
                                npm --version &&
                                nohup npm start > app.log 2>&1 &
                            '
                        """
                        // Wait for the application to start (adjust timeout if needed)
                        sh "sleep 30"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'This runs always after the pipeline is finished'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

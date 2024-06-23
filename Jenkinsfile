pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'mars-deploy' // Replace with your actual Jenkins credentials ID
        VPS_HOST = '185.199.53.88'       // Replace with your VPS IP or hostname
        VPS_USER = 'root'                // Replace with your VPS username
    }

    stages {
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'

                script {
                    // Use sshagent to load the SSH private key
                    sshagent(credentials: [env.VPS_CREDENTIALS]) {
                        // SSH commands to deploy the application
                        sh """
                            ssh ${env.VPS_USER}@${env.VPS_HOST} '
                                cd /home/marsinstitute/htdocs/www.marsinstitute.in/backend &&
                                source ~/.nvm/nvm.sh &&
                                nvm install 18.17.0 &&
                                nvm use 18.17.0 &&
                                echo \$PATH &&
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

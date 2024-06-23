pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'vps-ssh-key'  // Replace with your Jenkins credentials ID
        VPS_HOST = 'VPS_IP'              // Replace with your VPS IP or hostname
        VPS_USER = 'root'                // Replace with your VPS username
    }

    stages {
        stage('Deploy') {
            steps {
                sshagent(credentials: [env.VPS_CREDENTIALS]) {
                    script {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${env.VPS_USER}@${env.VPS_HOST} '
                                cd /home/marsinstitute/htdocs/www.marsinstitute.in/backend &&
                                source ~/.nvm/nvm.sh &&
                                nvm install 18.17.0 &&
                                nvm use 18.17.0 &&
                                echo \$PATH &&
                                npm --version &&
                                npm start
                            '
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }

        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}

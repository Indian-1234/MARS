pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'fd22bb75-c16e-479a-8eab-a3a27fa25746'  // Ensure this matches the credentials ID in Jenkins
        VPS_HOST = '185.199.53.88'              // Replace with your VPS IP or hostname
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


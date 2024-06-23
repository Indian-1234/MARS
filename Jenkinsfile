pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'mars-deploy' // Replace with your actual Jenkins credentials ID
        VPS_HOST = '185.199.53.88'       // Replace with your VPS IP or hostname
        VPS_USER = 'root'                // Replace with your VPS username
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                checkout scm // This assumes your Jenkinsfile is stored in the same repository you are checking out
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add your actual build steps here if needed
                // Example: mvn clean package
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

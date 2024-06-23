pipeline {
    agent any

    environment {
        VPS_CREDENTIALS = 'fd22bb75-c16e-479a-8eab-a3a27fa25746'       // Replace with your actual Jenkins credentials ID
        VPS_HOST = '185.199.53.88'            // Replace with your VPS IP or hostname
        VPS_USER = 'root'                     // Replace with your VPS username
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
                        // Kill any process running on port 8000
                        sh """
                            ssh -o StrictHostKeyChecking=no ${env.VPS_USER}@${env.VPS_HOST} '
                                fuser -k 8000/tcp || true
                            '
                        """
                        // Transfer build files to the VPS
                        sh """
                            scp -o StrictHostKeyChecking=no -r client/build/{asset-manifest.json,favicon.ico,index.html,manifest.json,robots.txt,static} root@185.199.53.88:/home/marsinstitute/htdocs/www.marsinstitute.in/MARS/client/build


                        """
                        // Connect to the VPS and start the application in the background
                        def sshCommand = '''
                            cd /home/marsinstitute/htdocs/www.marsinstitute.in/backend &&
                            source ~/.nvm/nvm.sh &&
                            kill-port 8000 &&
                            nvm use 18.17.0 &&
                            nohup npm start > app.log 2>&1 &
                        '''
                        // Run the SSH command in background
                        sh "ssh -o StrictHostKeyChecking=no ${env.VPS_USER}@${env.VPS_HOST} '${sshCommand}' > /dev/null 2>&1 &"
                    }
                }  
            }
        }

        stage('Wait for Deployment') {
            steps {
                echo 'Waiting for 10 seconds to allow the deployment to settle...'
                script {
                    sleep(time: 10, unit: 'SECONDS')
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

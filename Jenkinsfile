pipeline {
    agent any
    
    stages {
        stage('Deploy') {
            steps {
                script {
                    sshagent(credentials: ['fd22bb75-c16e-479a-8eab-a3a27fa25746']) {
                        // SSH commands here
                        sh '''
                           ssh root@185.199.53.88'
                               cd /home/marsinstitute/htdocs/www.marsinstitute.in/backend &&
                               npm install &&
                               npm start
                           '
                        '''
                    }
                }
            }
        }
    }
}

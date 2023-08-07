pipeline {
    agent {
        label 'aws'
    }

environment {
        REMOTE_USER = 'ec2-user'  
        REMOTE_HOST = '15.206.166.13'
        APP_DIRECTORY = '/var/www/html/'
        Repo_url = 'https://github.com/Akshatp32/Wordpress.git'
        SSH_CREDENTIALS = '38921be7-1df0-4485-a6cb-e730ea1d16bf' 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent(credentials: [${SSH_CREDENTIALS}]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} '
                                cd ${APP_DIRECTORY}
                                git pull ${Repo_url}
                                sudo systemctl restart httpd
                                sudo systemctl restart php-fpm
                            '
                        """
                    }
                }
            }
        }
    }
}

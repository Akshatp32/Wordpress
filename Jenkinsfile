pipeline {
    agent {
        label 'aws'
    }

environment {
        REMOTE_USER = 'ec2-user'  
        REMOTE_HOST = '15.206.166.13'
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
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} '
                                    sudo yum install httpd php* mariadb* -y &&
                                    sudo chown -R httpd:httpd /var/www/html &&
                                    git config --global --add safe.directory /var/www/html &&
                                    cd /var/www/html &&
                                    git pull https://github.com/Akshatp32/Wordpress.git &&
                                    sudo systemctl restart httpd &&
                                    sudo systemctl restart php-fpm
                            '
                        """
                    }
                }
            }
        }
    }
}

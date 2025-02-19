pipeline {
    agent any
    environment {
        SSH_CREDENTIALS_ID = 'schweppes' 
    }
    stages {
        stage('Install Apache on remote VM') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.SSH_CREDENTIALS_ID, usernameVariable: 'REMOTE_DEV_VM_USER', passwordVariable: 'REMOTE_DEV_VM_PASSWORD')]) {
                        sh """
                            ssh $REMOTE_DEV_VM_USER@$REMOTE_DEV_VM_IP 'echo $REMOTE_DEV_VM_PASSWORD | sudo -S apk add apache2 --no-interactive'
                        """
                    }
                }
            }
        }
        stage('Check Apache version') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.SSH_CREDENTIALS_ID, usernameVariable: 'REMOTE_DEV_VM_USER', passwordVariable: 'REMOTE_DEV_VM_PASSWORD')]) {
                        sh """
                            ssh $REMOTE_DEV_VM_USER@$REMOTE_DEV_VM_IP 'httpd -v'
                        """
                    }
                }
            }
        }
        stage('Check Apache logs') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.SSH_CREDENTIALS_ID, usernameVariable: 'REMOTE_DEV_VM_USER', passwordVariable: 'REMOTE_DEV_VM_PASSWORD')]) {
                        sh """
                            ssh $REMOTE_DEV_VM_USER@$REMOTE_DEV_VM_IP 'tail -n 100 /var/log/apache2/access.log | grep -E "4[0-9]{2}|5[0-9]{2}"'
                        """
                    }
                }
            }
        }
    }
}
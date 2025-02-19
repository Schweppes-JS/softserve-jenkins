pipeline {
    agent any
    environment {
        SSH_CREDENTIALS_ID = 'schweppes' 
    }
    stages {
        stage('Retrieve Credentials') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.SSH_CREDENTIALS_ID, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        env.REMOTE_DEV_VM_USER = USER
                        env.REMOTE_DEV_VM_PASSWORD = PASS
                    }
                }
            }
        }
        stage('Install Apache (httpd) Server') {
            steps {
                script {
                    sh """
                        ssh ${REMOTE_DEV_VM_USER}@${REMOTE_DEV_VM_IP} 'echo '${REMOTE_DEV_VM_PASSWORD}' | sudo -S apk add apache2 --no-interactive'
                    """
                }
            }
        }
        stage('Verify Apache Installation') {
            steps {
                script {
                    sh """
                        echo '${REMOTE_DEV_VM_PASSWORD}' | ssh ${REMOTE_DEV_VM_USER}@${REMOTE_DEV_VM_IP} 'httpd -v'
                    """
                }
            }
        }
        stage('Read Logs and Check for Errors') {
            steps {
                script {
                    sh """
                        echo '${REMOTE_DEV_VM_PASSWORD}' | ssh ${REMOTE_DEV_VM_USER}@${REMOTE_DEV_VM_IP} 'tail -n 100 /var/log/apache2/access.log | grep -E "4[0-9]{2}|5[0-9]{2}"'
                    """
                }
            }
        }
    }
}

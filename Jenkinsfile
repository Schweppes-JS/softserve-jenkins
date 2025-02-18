pipeline {
    agent any
    environment {
        REMOTE_VM_IP = 'your_vm_ip'
        REMOTE_VM_USER = 'your_vm_user'
        SSH_KEY_PATH = '/path/to/your/private/key'
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo "REMOTE_VM_IP: ${REMOTE_VM_IP}"
                echo "REMOTE_VM_USER: ${REMOTE_VM_USER}"
                echo "SSH_KEY_PATH: ${SSH_KEY_PATH}"
            }
        }
        stage('Install Apache (httpd) Server') {
            steps {
                script {
                    sshagent(credentials: ['your-ssh-key-credentials-id']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_VM_USER}@${REMOTE_VM_IP} 'sudo apt update && sudo apt install -y apache2'
                        """
                    }
                }
            }
        }
        stage('Verify Apache Installation') {
            steps {
                script {
                    sshagent(credentials: ['your-ssh-key-credentials-id']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_VM_USER}@${REMOTE_VM_IP} 'apache2 -v'
                        """
                    }
                }
            }
        }
        stage('Read Logs and Check for Errors') {
            steps {
                script {
                    sshagent(credentials: ['your-ssh-key-credentials-id']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_VM_USER}@${REMOTE_VM_IP} 'tail -n 100 /var/log/apache2/error.log | grep -E "4[0-9]{2}|5[0-9]{2}"'
                        """
                    }
                }
            }
        }
    }
}

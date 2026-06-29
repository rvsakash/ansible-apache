pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    echo "Current Branch: ${env.BRANCH_NAME}"
                    
                    // बिना किसी इफ-एल्स के सीधे आपकी प्लेबुक को रन करेंगे
                    sh "ansible-playbook -i inventory.ini deploy-apache.yml --vault-password-file vault_pass.txt"
                }
            }
        }
    }
}

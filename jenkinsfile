pipeline {
    agent any
    
    environment {
        // यह कमांड अपने आप पता लगा लेगी कि कोड किस रिपॉजिटरी से आया है
        REPO_NAME = "${env.GIT_URL.tokenize('/')[-1].replace('.git', '')}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // यह कमांड किसी भी रिपॉजिटरी की किसी भी ब्रांच को अपने आप पुल करेगी
                checkout scm
            }
        }
        
        stage('Run Ansible Playbook') {
            steps {
                script {
                    echo "Code pulled from Repository: ${REPO_NAME}"
                    echo "Current Branch: ${env.BRANCH_NAME}"
                    
                    // 1. अगर कोड apache वाली repo से आया है
                    if (REPO_NAME == 'ansible-apache') {
                        echo "Running Apache Server Setup..."
                        sh 'ansible-playbook install_apache.yml'
                    } 
                    // 2. अगर कोड पैचिंग वाली किसी भी repo से आया है
                    else if (REPO_NAME == 'ansible-patch-automation' || REPO_NAME == 'ansible-patch-repo') {
                        echo "Running OS Patching Automation..."
                        // यहाँ अपनी पैचिंग वाली प्लेबुक का सही नाम लिख देना भाई, जैसे patch.yml
                        sh 'ansible-playbook patch.yml' 
                    } 
                    // 3. अगर कोई मैच न मिले
                    else {
                        echo "No specific repository matched. Running default check."
                        sh 'ansible-playbook site.yml'
                    }
                }
            }
        }
    }
}

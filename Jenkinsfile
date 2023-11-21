pipeline {
    agent any
    environment {
        // Define the Vault password as an environment variable using the credential ID 'KubeConf'
        VAULT_PASS = credentials('KubeConf')
    }
    stages {
        stage('Deploy Nginx') {
            steps {
                script {
                    // Specify the absolute path to the playbook
                    def playbookPath = '/var/lib/jenkins/workspace/Ansible/playbooks/Nginx-deployment.yaml'

                    // Run the Ansible playbook with the Vault password
                    sh "ansible-playbook $playbookPath --vault-password-file=<(echo $VAULT_PASS)"
                }
            }
        }
    }
}

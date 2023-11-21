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

                    // Write the Vault password to a temporary file
                    def vaultPassFile = 'vault_pass.txt'
                    writeFile file: vaultPassFile, text: env.VAULT_PASS

                    try {
                        // Run the Ansible playbook using the temporary file for the Vault password
                        sh "ansible-playbook ${playbookPath} --vault-password-file ${vaultPassFile}"
                    } finally {
                        // Clean up the temporary file
                        sh "rm -f ${vaultPassFile}"
                    }
                }
            }
        }
    }
}

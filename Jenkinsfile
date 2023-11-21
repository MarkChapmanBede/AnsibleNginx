pipeline {
    agent any
    environment {
        // Define the Vault password as an environment variable using the credential ID 'KubeConf'
        VAULT_PASS = credentials('KubeConf')
        // Assuming 'KUBECONFIG_FILE' is the ID of the kubeconfig file stored in Jenkins credentials
        KUBECONFIG_FILE = credentials('KUBECONFIG_FILE')
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
        stage('Deploy Prometheus') {
            steps {
                script {
                    // Add Prometheus Helm repository
                    sh 'helm repo add prometheus-community https://prometheus-community.github.io/helm-charts'
                    sh 'helm repo update'

                    // Write kubeconfig to a temporary file
                    def kubeconfigFile = 'kubeconfig'
                    writeFile file: kubeconfigFile, text: env.KUBECONFIG_FILE

                    try {
                        // Install or upgrade Prometheus using the kubeconfig file
                        sh "KUBECONFIG=${kubeconfigFile} helm upgrade --install prometheus prometheus-community/prometheus"
                    } finally {
                        // Clean up the kubeconfig file
                        sh "rm -f ${kubeconfigFile}"
                    }
                }
            }
        }
    }
}

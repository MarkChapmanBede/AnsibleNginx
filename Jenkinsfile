pipeline {
    agent any
    environment {
        // Define the Vault password as an environment variable using the credential ID 'KubeConf'
        VAULT_PASS = credentials('KubeConf')
        // Set the path to your kubeconfig file on the Jenkins server
        KUBECONFIG_PATH = '/home/ubuntu/.kube/config'
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

                    // Install or upgrade Prometheus using the kubeconfig file
                    sh "KUBECONFIG=${KUBECONFIG_PATH} helm upgrade --install prometheus prometheus-community/prometheus"
                }
            }
        }
    }
}

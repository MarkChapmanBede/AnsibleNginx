pipeline {
    agent any

    stages {
        stage('Deploy Nginx') {
            steps {
                script {
                    // Specify the absolute path to the playbook and inventory file
                    def playbookPath = '/var/lib/jenkins/workspace/Ansible/playbooks/Nginx-deployment.yaml'
                    def inventoryPath = '-i localhost'

                    // Run the Ansible playbook with absolute paths
                    sh "ansible-playbook $inventoryPath $playbookPath"
                }
            }
        }
    }
}

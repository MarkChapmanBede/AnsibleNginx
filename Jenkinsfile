pipeline {
    agent any

        stage('Deploy Nginx') {
            steps {
                script {
                    // Run the Ansible playbook to deploy Nginx
                    sh 'ansible-playbook /playbooks/Nginx-deployment.yaml'
                }
            }
        }
    }
}

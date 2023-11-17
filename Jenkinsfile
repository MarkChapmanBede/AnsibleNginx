pipeline {
    agent any

    stages {
        stage('Deploy Nginx') {
            steps {
                script {
                    // Run the Ansible playbook to deploy Nginx
                    sh 'ansible-playbook /var/lib/jenkins/workspace/Ansible/playbooks/Nginx-deployment.yaml'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = 'Ansible1.yml'
    }

    parameters {
        string(name: 'Blade', defaultValue: '', description: 'Blade number (e.g., 7, 10)')
        string(name: 'Fase', defaultValue: '', description: 'Fase (e.g., O, T, A, P)')
    }

    stages {
        stage('Preparation') {
            steps {
                // Check out the repository
                checkout scm
                
                // Ensure Ansible is installed
                script {
                    def ansibleInstalled = sh(script: 'which ansible', returnStatus: true) == 0
                    if (!ansibleInstalled) {
                        error "Ansible is not installed on the Jenkins agent. Please install Ansible."
                    }
                }

                // Install necessary Ansible modules
                sh 'ansible-galaxy collection install community.vmware'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Execute the Ansible playbook with the specified variables
                    sh "ansible-playbook ${ANSIBLE_PLAYBOOK} -e \"Blade=${params.Blade} Fase=${params.Fase}\""
                }
            }
        }
    }

    post {
        success {
            echo 'Ansible playbook executed successfully.'
        }
        failure {
            echo 'Ansible playbook execution failed.'
        }
    }
}

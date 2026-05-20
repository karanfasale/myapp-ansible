pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test Ansible Connectivity') {
            steps {
                sh 'ansible -i inventory.ini webservers -m ping'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                ansiblePlaybook(
                    playbook: 'site.yml',
                    inventory: 'inventory.ini',
                    credentialsId: 'app-servers-ssh-key',
                    colorized: true,
                    disableHostKeyChecking: true,
                    extras: '-v'
                )
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! App is live.'
        }
        failure {
            echo 'Deployment failed. Check the Ansible output above.'
        }
    }
}

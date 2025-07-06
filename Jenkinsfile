pipeline {
    agent any

    stages {
        stage('Checkout Done') {
            steps {
                echo 'Repository already checked out by Jenkins.'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }
}

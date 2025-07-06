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
                sh '''
                python3 -m venv venv
                . ./venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                . ./venv/bin/activate
                ansible-playbook -i inventory.ini deploy.yml
                '''
            }
        }
    }
}

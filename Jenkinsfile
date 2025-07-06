pipeline {
  agent any

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/hussaindenish/flask-jenkins-ansible-Project.git'
      }
    }

    stage('Deploy with Ansible') {
      steps {
        sh 'ansible-playbook -i inventory.ini deploy.yml'
      }
    }
  }
}


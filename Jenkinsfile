pipeline {
  agent any

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/hussaindenish/ci-cd-pipeline.git'
      }
    }

    stage('Deploy with Ansible') {
      steps {
        sh 'ansible-playbook -i inventory.ini deploy.yml'
      }
    }
  }
}


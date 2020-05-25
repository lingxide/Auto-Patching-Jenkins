pipeline {
  agent any
  stages {
    stage('Ready') {
      steps {
        sh 'pwd'
        echo 'Just printed current folder'
      }
    }

    stage('adhoc command') {
      steps {
        sh 'whoami'
      }
    }

    stage('Run Playbook') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/patching.yml', colorized: true, inventory: '/mnt/xfer/ansible/hosts', become: true, becomeUser: 'root', extras: '--private-key "/opt/patch-keys/id_rsa"')
      }
    }

  }
}
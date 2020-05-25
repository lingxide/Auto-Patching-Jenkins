pipeline {
  agent any
  stages {
    stage('Ready') {
      steps {
        sh 'pwd'
        echo 'Just printed current folder'
      }
    }

    stage('Sleep') {
      steps {
        sleep 5
      }
    }

    stage('uptime') {
      steps {
        retry(count: 3) {
          timeout(time: 10) {
            sh 'w'
            sleep 3
          }

        }

      }
    }

    stage('Run Playbook') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/patching.yml', colorized: true, inventory: '/mnt/xfer/ansible/hosts', become: true, becomeUser: 'root', extras: 'ansible_ssh_private_key_file="/opt/patch-keys/id_rsa"')
      }
    }

  }
}
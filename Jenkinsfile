pipeline {
  agent any
  stages {
    stage('Env Setup') {
      steps {
        sh 'pwd'
        sh 'sudo mount -a'
      }
    }

    stage('Adhoc Command') {
      steps {
        sh 'whoami'
      }
    }

    stage('Server Precheck') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/precheck.yml', colorized: true, inventory: '/mnt/xfer/ansible/hosts', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', disableHostKeyChecking: true)
      }
    }

    stage('OS Patching') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/patching.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/hosts')
      }
    }

    stage('Server Reboot') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/reboot.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/hosts')
        retry(count: 20) {
          sleep 5
          ansiblePlaybook(playbook: '/mnt/xfer/ansible/keepalive.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/hosts')
        }

      }
    }

    stage('Server Postcheck') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/postcheck.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/hosts')
      }
    }

    stage('Business Verification') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/verify.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/hosts')
      }
    }

  }
}

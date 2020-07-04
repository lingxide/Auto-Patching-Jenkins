pipeline {
  agent any
  stages {
    stage('Local Env Setup') {
      steps {
        sh 'pwd'
        sh 'sudo mount -a'
      }
    }

    stage('Adhoc Command') {
      steps {
        echo 'Build Live Server List'
        sh 'bash /mnt/xfer/ansible/pinglive.sh'
      }
    }

    stage('Server Precheck') {
      steps {
        echo 'Precheck on Centos VM'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/precheck_centos.yml', colorized: true, inventory: '/mnt/xfer/ansible/centos_host.txt', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', disableHostKeyChecking: true)
        echo 'Precheck on Debian VM'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/precheck_debian.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/debian_host.txt')
      }
    }

    stage('OS Patching') {
      steps {
        echo 'Patching Centos Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/patching.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/centos_host.txt')
        echo 'Patching Debian Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/patching.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/debian_host.txt')
      }
    }

    stage('Server Reboot') {
      steps {
        echo 'Reboot Centos Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/reboot.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/centos_host.txt')
        echo 'Reboot Debian Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/reboot.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/debian_host.txt')
      }
    }

    stage('Server Postcheck') {
      steps {
        echo 'Wait Till Server Alive'
        sleep 20
        echo 'Postcheck on Centos Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/postcheck.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/centos_host.txt')
        echo 'Postcheck on Debian Server'
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/postcheck.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/debian_host.txt')
      }
    }

    stage('Business Verification') {
      steps {
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/verify.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/centos_host.txt')
        ansiblePlaybook(playbook: '/mnt/xfer/ansible/verify.yml', extras: '-u root --private-key "/opt/patch-keys/id_rsa" -vvv', inventory: '/mnt/xfer/ansible/debian_host.txt')
      }
    }

  }
}
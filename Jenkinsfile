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

    stage('') {
      steps {
        retry(count: 3) {
          timeout(time: 10) {
            sh 'w'
            sleep 3
          }

        }

      }
    }

  }
}
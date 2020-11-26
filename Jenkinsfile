pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('error') {
      steps {
        sh 'sh test.sh'
      }
    }

  }
}
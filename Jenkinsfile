pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
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
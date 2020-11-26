pipeline {
  agent any
  stages {
    stage('exec shell') {
      steps {
        sh '''sh test.sh
cat Jenkinsfile'''
      }
    }

    stage('') {
      steps {
        readFile(file: 'Jenkinsfile', encoding: 'UTF-8')
      }
    }

  }
}
pipeline {
  agent any
  stages {
    stage('exec shell') {
      steps {
        sh '''sh test.sh
cat Jenkinsfile'''
      }
    }

  }
}
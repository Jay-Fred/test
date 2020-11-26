pipeline {
  agent none
  stages {
    stage('error') {
      steps {
        git(url: 'git@github.com:Jay-Fred/test.git', branch: 'master', poll: true, changelog: true)
      }
    }

  }
}
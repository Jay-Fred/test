pipeline {
  agent any
  stages {
    stage('exec shell') {
      steps {
        sh '''sh test.sh
cat Jenkinsfile'''
      }
    }

    stage('write_file') {
      parallel {
        stage('write_file') {
          steps {
            writeFile(file: 'new_test', text: 'this is second file', encoding: 'UTF-8')
          }
        }

        stage('error') {
          steps {
            sh 'sed -i \'s#second#uuuuuuuuuuu#g\' new_test'
          }
        }

      }
    }

    stage('ehco_newfile') {
      steps {
        sh '''cat new_test
cat test.sh'''
      }
    }

  }
}
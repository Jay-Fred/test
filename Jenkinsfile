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

        stage('') {
          steps {
            sh 'sed -i \'s#test#uuuuuuuuuuu#g\' test.sh'
          }
        }

      }
    }

    stage('ehco_newfile') {
      steps {
        sh '''cat new_test
echo -e "\\n"
cat test.sh'''
      }
    }

  }
}
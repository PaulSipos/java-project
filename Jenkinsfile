pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        echo 'Building..'
        sh 'ant -f build.xml -v'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing..'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying..'
      }
    }
  }

}

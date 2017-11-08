pipeline {
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '5',artifactNumToKeepStr: '3'))
  }

  stages {
    stage('Build') {
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

  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

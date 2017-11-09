pipeline {
  agent any


  stages {
    stage('Unit Tests'){
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
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

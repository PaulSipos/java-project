pipeline {
  agent {
    label 'master'
  }


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

    stage('Deploy') {
      steps {
        echo 'Deploying..'
        sh "cp dist/Rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

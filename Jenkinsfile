pipeline {
  agent none


  stages {
    stage('Unit Tests'){
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('Build') {
      agent {
        label 'apache'
      }
      steps {
        echo 'Building..'
        sh 'ant -f build.xml -v'
      }
    }

    stage('Deploy') {
      agent {
        label 'apache'
      }
      steps {
        echo 'Deploying..'
        sh "cp dist/Rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
    stage ("Running on CentOS") {
      agent {
        label 'CentOS'
      }
      steps {
        sh "wget http://jenkins.local:8081/rectangles/all/Rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar Rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

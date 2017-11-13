pipeline {
  agent none
  environment {
    MAJOR_VERSION=1
  }

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
      post {
        success {
          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
      }
    }

    stage('Deploy') {
      agent {
        label 'apache'
      }
      steps {
        echo 'Deploying..'
        sh "mkdir -p /var/www/html/rectangles/all/${env.BRANCH_NAME}"
        sh "cp dist/Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }
    stage ("Running on CentOS") {
      agent {
        label 'CentOS'
      }
      steps {
        sh "wget http://192.168.105.30:8081/rectangles/all/${env.BRANCH_NAME}/Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage ("Test on Debian") {
      agent {
        docker 'openjdk:8u151-jre'
      }
      steps {
        sh "hostname"
                sh "wget http://192.168.105.30:8081/rectangles/all/${env.BRANCH_NAME}/Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
        sh "java -jar Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    stage('Promote to Green'){
      agent {
        label 'apache'
      }
      when {
        branch 'master'
      }
      steps {
        sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/Rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/"
      }
    }
    stage ('Promote Development Branch to Master') {
      agent {
        label 'apache'
      }
      when {
        branch 'development'
      }
      steps {
        echo 'Stashing Any Local Changes'
        sh 'git stash'
        echo 'Checking Out Development Branch'
        sh 'git checkout development'
        echo 'Checking Out the Master Branch'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge development'
        echo 'Pushing to Origin Master...'
        sh 'git push origin master'
        echo 'Tagging the release..'
        sh "git tag Rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
        sh "git push origin Rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
      }
    }

  }
}

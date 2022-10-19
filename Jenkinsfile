pipeline {
  agent {
    docker {
      image 'node:17-alpine'
      args '-p 3000:3000'
    }
  }
  environment {
    CI = 'true'
  }
  stages {
    stage("Config") {
      steps {
        sh 'npm config get registry'
        sh 'npm config rm proxy'
        sh 'npm config rm https-proxy'
        sh 'npm config set registry http://registry.npmjs.org/'
      }
    }
    stage("Build") {
      steps {
        sh 'npm install'
      }
    }
    stage("Test") {
      steps {
        sh 'jenkins/scripts/test.sh'
      }
    }
    stage("Deliver") {
      steps {
        sh 'jenkins/scripts/deliver.sh'
        input message: 'Finished using the web site ? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
  }
}

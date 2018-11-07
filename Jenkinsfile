pipeline {
  agent {
    node {
      label 'LIN_WKR'
    }

  }
  stages {
    stage('Infrastructure') {
      steps {
        sh '''sudo apt-get -y install build-essential automake autoconf-archive pkg-config libtool autoconf autotools-dev gdb valgrind uuid-dev bison flex python-docutils 
'''
        sh '''sudo add-apt-repository ppa:adiscon/v8-stable -y
'''
        sh '''sudo apt-get update -qq
'''
        sh '''sudo apt-get install -y -qq clang
'''
        sh 'sudo apt-get install -y -qq --force-yes libestr-dev libfastjson-dev liblogging-stdlog-dev'
      }
    }
    stage('Build') {
      steps {
        sh 'autoreconf -fvi'
        sh './configure --enable-tls-openssl'
        sh 'make'
      }
    }
    stage('Static Analysis') {
      parallel {
        stage('Static Analysis') {
          steps {
            echo 'SA'
          }
        }
        stage('SAST') {
          steps {
            echo 'SAST'
          }
        }
        stage('Source Clearing') {
          steps {
            echo 'Clearing'
            catchError() {
              sh 'ls -al'
            }

          }
        }
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploy'
      }
    }
  }
}
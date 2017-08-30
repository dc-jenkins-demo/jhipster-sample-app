pipeline {
  agent {
    docker {
      image 'rowanto/docker-java8-mvn-nodejs-npm'
    }
    
  }
  stages {
    stage('Checkout') {
      steps {
        script {
          checkout scm
        }
        
        stash(name: 'ws', includes: '**')
      }
    }
    stage('Build Backend') {
      steps {
        unstash 'ws'
        sh 'mvn -B -DskipTests=true clean compile package'
        stash(name: 'war', includes: 'target/**/*.war')
      }
    }
  }
}
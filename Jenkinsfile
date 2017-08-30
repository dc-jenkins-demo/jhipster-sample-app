pipeline {
  agent {
    docker {
      image 'rowanto/docker-java8-mvn-nodejs-npm'
    }
    
  }
  stages {
    stage('Checkout') {
      steps {
        stash(includes: '**', name: 'ws')
      }
    }
    stage('Build Backend') {
      steps {
        unstash 'ws'
        sh './mvnw -B -DskipTests=true clean compile package'
        stash(name: 'war', includes: 'target/**/*.war')
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit": {
            unstash 'ws'
            unstash 'war'
            sh './mvnw -B test'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Performance": {
            unstash 'ws'
            unstash 'war'
            sh './mvnw -B gatling:execute'
            
          }
        )
      }
    }
    stage('Test Frontend') {
      steps {
        unstash 'ws'
        sh 'yarn install'
        sh 'yarn global add gulp-cli'
        sh 'gulp test'
      }
    }
    stage('Build Container') {
      steps {
        unstash 'ws'
        unstash 'war'
        sh './mvnw -B docker:build'
      }
    }
    stage('Deploy to Staging') {
      steps {
        echo 'Let\'s pretend a deployment is happening'
      }
    }
    stage('Deploy to production') {
      steps {
        input(message: 'Deploy to production?', ok: 'Fire zee missiles!')
        echo 'Let\'s pretend a production deployment is happening'
      }
    }
  }
}
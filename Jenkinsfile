pipeline {
  agent {
    docker {
      image 'rowanto/docker-java8-mvn-nodejs-npm'
    }
    
  }
  stages {
    stage('Build Backend') {
      steps {
        echo 'sh ./mvnw -B -DskipTests=true package'
        stash(name: 'war', includes: 'target/**/*.war')
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit": {
            echo 'running: sh ./mvnw -B test'
            echo 'running: junit **/surefire-reports/**/*.xml'
            
          },
          "Performance": {
            echo 'running: sh ./mvnw -B gatling:execute'
            
          }
        )
      }
    }
    stage('Test Frontend') {
      steps {
        echo 'Running npm build'
      }
    }
    stage('Build Container') {
      steps {
        echo 'Running container build'
      }
    }
    stage('Deploy war to Staging') {
      steps {
        unstash 'war'
        echo 'Deploying to a test server'
      }
    }
    stage('Deploy to production') {
      steps {
        input(message: 'Deploy to production?', ok: 'Go!')
        unstash 'war'
        echo 'Production deployed'
      }
    }
  }
}
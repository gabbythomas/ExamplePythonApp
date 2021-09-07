pipeline {
  agent { label 'python-build' }
  stages {
    stage('Preparation') {
      steps {
        checkout scm
      }
    }
    
    stage('Build target image') {
      steps {
        docker.build('example-python-app', 'Dockerfiles/build-app.dockerfile')
      }
    }

    stage('Smoke test target image') {
      steps {
        sh 'echo testing...'
      }
    }

    stage('Push target image to Dockerhub') {
      steps {
        sh 'echo pushing...'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}

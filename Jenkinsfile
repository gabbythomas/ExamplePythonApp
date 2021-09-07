pipeline {
  agent python-build
  stages {
    stage('Build target image') {
      steps {
        sh 'echo building...'
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

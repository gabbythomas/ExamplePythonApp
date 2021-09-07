pipeline {
  agent { label 'python-build' }
  
  environment {
    BUILD_IMAGE_NAME = 'example-python-app-build'
  }
  
  stages {
    stage('Preparation') {
      steps {
        checkout scm
      }
    }
    
    stage('Build target image') {
      steps {
        script {
          docker.build($BUILD_IMAGE_NAME, '-f Dockerfiles/build-app.dockerfile .')
        }
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

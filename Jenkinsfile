pipeline {
  agent { label 'python-build' }
  
  environment {
    BUILD_IMAGE_NAME = "example-python-app-build/${BRANCH_NAME}:${BUILD_NUMBER}"
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
          docker.build('${BUILD_IMAGE_NAME}', '-f Dockerfiles/build-app.dockerfile .')
        }
      }
    }

    stage('Smoke test target image') {
      steps {
        script {
          docker.image('${BUILD_IMAGE_NAME}').withRun('-p 5000:5000') {
            sh 'curl localhost:5000'
          }
        }
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
      
      script {
          sh 'docker image rm ${BUILD_IMAGE_NAME}'
      }
    }
  }
}

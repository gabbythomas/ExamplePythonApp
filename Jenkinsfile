pipeline {
  agent { label 'python-build' }
  
  environment {
    BUILD_IMAGE_NAME = "example-python-app-build/${BRANCH_NAME}:${BUILD_NUMBER}"
    TEST_IMAGE_NAME = "example-python-app-test/${BRANCH_NAME}:${BUILD_NUMBER}"
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
          docker.build('${TEST_IMAGE_NAME}', '-f Dockerfiles/test-app.dockerfile .')
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
        sh 'docker image rm ${TEST_IMAGE_NAME}'
      }
    }
  }
}

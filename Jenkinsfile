pipeline {
  agent { label 'python-build' }
  
  environment {
    BUILD_IMAGE_NAME = "example-python-app-build/${BRANCH_NAME}:${BUILD_NUMBER}"
    BUILD_CONTAINER_NAME = "example-python-app-${BRANCH_NAME}-${BUILD_NUMBER}"
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
          sh 'docker run -dit -p 5000:5000 --name ${BUILD_CONTAINER_NAME} ${BUILD_IMAGE_NAME}'
          sh 'curl http://localhost:5000'
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
          sh 'docker container rm -f ${BUILD_CONTAINER_NAME}'
          sh 'docker image rm ${BUILD_IMAGE_NAME}'
      }
    }
  }
}

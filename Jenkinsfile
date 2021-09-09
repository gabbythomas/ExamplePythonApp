pipeline {
  agent { label 'python-build' }
  
  environment {
    BUILD_IMAGE_NAME = "example-python-app-build/${BRANCH_NAME}:${BUILD_NUMBER}"
    TEST_IMAGE_NAME = "example-python-app-test/${BRANCH_NAME}:${BUILD_NUMBER}"
    RELEASE_IMAGE_NAME = "docker://docker.io/gabrient/example-python-app:${BUILD_NUMBER}"
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
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials',
                                            usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            sh 'docker login -u ${USER} -p ${PASS} docker.io'
            sh 'docker push ${BUILD_IMAGE_NAME} ${RELEASE_IMAGE_NAME}'
          }
        }
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

pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        checkout scm
        sh '''
          docker build -t $IMAGE_NAME:$BUILD_ID -f app-example/Dockerfile app-example
        '''
      }
    }
    stage('Test') {
      steps {
        echo 'TODO: add tests'
      }
    }
    stage('Image Release') {
      steps {
        sh '''
	  docker push $IMAGE_NAME:$BUILD_ID
	'''
      }
    }
    stage('Production Deployment') {
      steps {
        sh '''
          sed \'s#\'$IMAGE_NAME\':latest#\'$IMAGE_NAME:$BUILD_ID\'#\' app-example/k8s/deployment.yaml | kubectl app-examplely -f -
          kubectl rollout status deployment/app-example
        '''
      }
    }
  }
  environment {
    IMAGE_NAME = 'localhost:30000/app-example'
  }
}

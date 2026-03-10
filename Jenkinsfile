pipeline {
  agent any

  environment {
    DOCKERHUB_USER = "blacknight42607"
    IMAGE_NAME     = "simple-app"
    IMAGE_TAG      = "${env.BUILD_NUMBER}"
    LATEST_TAG     = "latest"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker --version
          docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
          docker tag ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${DOCKERHUB_USER}/${IMAGE_NAME}:${LATEST_TAG}
        '''
      }
    }

    stage('Login to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          sh '''
            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
          '''
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        sh '''
          docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
          docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${LATEST_TAG}
        '''
      }
    }
  }
}
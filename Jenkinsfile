pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'yourdockerhubusername/your-image-name'
    DOCKER_TAG = 'latest'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-username/your-repo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          sh "docker build -t $DOCKER_IMAGE:$DOCKER_TAG ."
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
          )]) {
            sh '''
              echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
              docker push $DOCKER_IMAGE:$DOCKER_TAG
            '''
          }
        }
      }
    }
  }

  post {
    success {
      echo 'Docker image built and pushed successfully!'
    }
    failure {
      echo 'Something went wrong 😢'
    }
  }
}

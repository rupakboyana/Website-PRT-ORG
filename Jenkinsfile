pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    DOCKER_IMAGE = 'your-dockerhub-username/website-prt-org'
  }
  stages {
    stage('Build') {
      steps {
        script {
          docker.build(DOCKER_IMAGE)
        }
      }
    }
    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
            docker.image(DOCKER_IMAGE).push('latest')
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        sh 'kubectl apply -f k8s-deployment.yaml'
        sh 'kubectl apply -f k8s-service.yaml'
      }
    }
  }
  triggers {
    githubPush()
  }
}

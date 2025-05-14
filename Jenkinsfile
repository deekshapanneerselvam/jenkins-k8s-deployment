pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'deekshapanneerselvam/node-app:latest'
  }

  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/deekshapanneerselvam/jenkins-k8s-deployment.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $DOCKER_IMAGE ./app'
      }
    }

    stage('Push Image to DockerHub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push $DOCKER_IMAGE'
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f kubernetes/deployment.yaml'
      }
    }
  }
}

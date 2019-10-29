pipeline {
  agent any
  stages {
    stage('Prep') {
      steps {
        sh '''kubectl delete deploy hw2-deployment
kubectl delete svc hw2-svc'''
        dir(path: '/home/ubuntu/cicd/git-code') {
          sh 'git pull'
        }

      }
    }
    stage('Build') {
      steps {
        dir(path: '/home/ubuntu/cicd/git-code') {
          sh 'jar -cvf WebApp1.war *  '
        }

        sh 'cp /home/ubuntu/cicd/git-code/WebApp1.war /home/ubuntu/cicd/image-build'
      }
    }
    stage('Package') {
      steps {
        dir(path: '/home/ubuntu/cicd/image-build') {
          sh 'cp /home/ubuntu/cicd/git-code/WebApp1.war .'
          sh 'docker build -t swe645:hw2 .'
        }

      }
    }
    stage('Deploy') {
      steps {
        dir(path: '/home/ubuntu/cicd/kub-deploy') {
          sh 'kubectl apply -f hw2-dep.yaml'
          sh 'kubectl apply -f hw2-svc.yaml'
        }

      }
    }
  }
}
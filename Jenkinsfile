pipeline {
  agent any
  stages {
    stage('build') {
        steps {
            sh 'docker build -t registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER .' 
        }
    }

    stage('push') {
      steps {
        withDockerRegistry([credentialsId: 'dockerhub', url: 'https://registry.cn-hangzhou.aliyuncs.com']) {
            sh 'docker push registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER'
        }
      }
    }
  }
}
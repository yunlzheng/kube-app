pipeline {
  agent any
  stages {
    stage('build') {
        steps {
            sh 'docker build -t registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER .' # 修改为自己的镜像
        }
    }

    stage('push') {
        withDockerRegistry([credentialsId: 'dockerhub', url: 'registry.cn-hangzhou.aliyuncs.com']) {
            sh 'docker push registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER' #修改为自己的镜像
        }
    }
  }
}
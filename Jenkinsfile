pipeline {
  agent any

  environment {
    HELM_USERNAME = credentials('HELM_USERNAME')
    HELM_PASSWORD = credentials('HELM_PASSWORD')
  }

  stages {
    stage('build') {
        steps {
            sh 'docker build -t registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER .' 
        }
    }

    stage('push image') {
      steps {
        withDockerRegistry([credentialsId: 'dockerhub', url: 'https://registry.cn-hangzhou.aliyuncs.com']) {
            sh 'docker push registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/kube-app:$BUILD_NUMBER'
        }
      }
    }

    stage('push chart') {

      steps {
        script {
            def filename = 'helm/kube-app/values.yaml'
            def data = readYaml file: filename
            data.image.tag = env.BUILD_NUMBER
            sh "rm $filename"
            writeYaml file: filename, data: data
        }

        script {
            def filename = 'helm/kube-app/Chart.yaml'
            def data = readYaml file: filename
            data.version = env.BUILD_NUMBER
            sh "rm $filename"
            writeYaml file: filename, data: data
        }

        sh 'helm push helm/kube-app https://repomanage.rdc.aliyun.com/helm_repositories/1148-test --username=$HELM_USERNAME --password=$HELM_PASSWORD  --version=$BUILD_NUMBER'
      }
    }

    stage('Deploy To Dev') {
      steps {
            input 'Do you approve dev?'
            sh 'helm upgrade kube-app-dev --install --namespace=yunlong helm/kube-app'
        }
    }

  }
}
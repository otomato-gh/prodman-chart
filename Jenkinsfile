pipeline {
  agent { kubernetes {
      label 'releaser'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    product: prodman
spec:
  containers:
  - name: helm
    image: dtzar/helm-kubectl
    imagePullPolicy: Always
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('deploy-to-staging') {
      steps {
        container('helm'){
          sh "helm install ./chart --name prodman-${BUILD_NUMBER} --namespace staging"
        }
      }
    }
  }
}

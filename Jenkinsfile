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
          sh "helm upgrade -i prodman ./chart --namespace staging"
        }
      }
    }
    stage('smoke') {
     steps {
       sh './smoke.sh'
     }
    }
    stage('performance-test') {
      steps {
       sh 'perf-test.sh'
      }
    }

    stage('deploy-to-prod') {
      steps {
        container('helm'){
          sh "helm upgrade -i prodman-prod ./chart --namespace production"
        }
      }
    }
  }
}

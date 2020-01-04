pipeline {

  environment {
    registry = "68.183.91.139:5000/myweb"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/vrushali-rajpure/dobby.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "examples/kubernetes/deployment.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
  }
}
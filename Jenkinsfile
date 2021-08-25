pipeline {
  environment {
    imagename = "steven8519/developershubmaster"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent any
  stages {
    stage('Chechout') {
      steps {
        git([url: 'https://github.com/Steven8519/developershub-client.git', branch: 'master', credentialsId: 'github'])

      }
    }

    stage('Build') {
        steps{
           script {
               sh "npm install"
               sh "npm run build"
            }
        }
    }

    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }

    stage("Deploy To Kuberates Cluster") {
       steps {
          sh 'kubectl replace -f deployment.yml'
        }
     }

    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
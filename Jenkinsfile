
pipeline {
  agent any
  stages {
    stage('Hello') {
      steps {
        echo 'Hello, Jenkins!'
      }
    }
  }
}
pipeline {

  agent any
 
  environment {

    REGISTRY_CREDENTIAL = 'dockerhub-creds'

    IMAGE_NAME = 'ezekielwilson/zenuss' // Replace with your Docker Hub repo name

  }
 
  stages {

    stage('Checkout') {

      steps {

        git url: 'https://github.com/Ezekiel-redhat/zenuss.git', branch: 'pipeline-setup'

      }

    }
 
    stage('Build Docker Image') {

      steps {

        script {

          docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")

        }

      }

    }
 
    stage('Push to Docker Hub') {

      steps {

        script {

          docker.withRegistry('', REGISTRY_CREDENTIAL) {

            docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()

            docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push('latest')

          }

        }

      }

    }
 
    stage('Clean Up') {

      steps {

        sh "docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || true"

        sh "docker rmi ${IMAGE_NAME}:latest || true"

      }

    }

  }
 
  post {

    always {

      echo 'Pipeline finished!'

    }

  }

}

 

pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }
   stages {
   stage('Building image') {
      steps{
          sh '''
         sudo docker build -t testapp .
             '''  
        }
    }
  
  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
   stage('Deploy Image') {
      steps{
script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh "echo ${DOCKER_HUB_PASSWORD} | sudo docker login -u ${DOCKER_HUB_USERNAME} --password-stdin http://192.168.0.210:8083"
                        sh "sudo docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} 192.168.0.210:8083/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                        sh "sudo docker push 192.168.0.210:8083/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                    }
        }
      }
           
        }
    }
}


    
  


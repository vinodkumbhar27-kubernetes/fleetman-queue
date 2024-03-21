pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME = "fleetman-queue"
     REPOSITORY_TAG = "${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}".toLowerCase()
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''echo No build required for Queue'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh '''
              docker image build -t ${REPOSITORY_TAG} .
              wget --no-check-certificate -O activemq.tar.gz http://archive.apache.org/dist/activemq/5.14.3/apache-activemq-5.14.3-bin.tar.gz
           '''
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh "envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -"
          }
      }
   }
}

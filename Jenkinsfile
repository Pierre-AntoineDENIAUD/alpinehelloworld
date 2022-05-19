pipeline {
     environment {
       ID_DOCKER = "opssuperieurdev"
       IMAGE_NAME = "pipeline"
       IMAGE_TAG = "latest"
       STAGING = "${ID_DOCKER}-staging"
       PRODUCTION = "${ID_DOCKER}-production"
     }
     agent none
     stages {
         stage('Build image') {
             agent any
             steps {
                script {
                  sh 'docker build -t ${votre_id_dockerhub}/${IMAGE_NAME}:${IMAGE_TAG} .'
                }
             }
        }
        stage('Run container based on builded image') {
            agent any
            steps {
               script {
                 sh 'curl http://172.17.0.1 | grep -q "Hello world!"'

               }
            }
       }
       stage('Test image') {
           agent any
           steps {
              script {
                sh 'curl http://172.17.0.1 | grep -q "Hello world!"'
               
              }
           }
      }
      stage('Clean Container') {
          agent any
          steps {
             script {
               sh '''

               '''
             }
          }
     }

     stage ('Login and Push Image on docker hub') {
          agent any
          steps {
             script {
               sh '''

               '''
             }
          }
      }    
     
     stage('Push image in staging and deploy it') {
       when {
              expression { GIT_BRANCH == 'origin/main' }
            }
      agent any
      environment {
          HEROKU_API_KEY = credentials('heroku_api_key')
      }  
      steps {
          script {
            sh '''
              heroku container:login
              heroku create $STAGING || echo "project already exist"
              heroku container:push -a $STAGING web
              heroku container:release -a $STAGING web
            '''
          }
        }
     }



     stage('Push image in production and deploy it') {
       when {
              expression { GIT_BRANCH == 'origin/main' }
            }
      agent any
      environment {
          HEROKU_API_KEY = credentials('heroku_api_key')
      }  
      steps {
          script {
            sh '''
              heroku container:login
              heroku create $PRODUCTION || echo "project already exist"
              heroku container:push -a $PRODUCTION web
              heroku container:release -a $PRODUCTION web
            '''
          }
        }
     }
  }
}
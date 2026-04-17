pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
         // Use bat for Windows instead of sh
         bat 'docker build -t azure-voting-app-redis ./azure-vote'
         }
      }
      stage('Cleanup Old Container') {
        steps {
            bat 'docker rm -f azure-voting-app-redis || true'
        }
      }
      stage('Start App')  {
         steps {
         // Use bat for Windows instead of sh
         bat 'docker run -d --rm -p 80:80 --name azure-voting-app-redis azure-voting-app-redis'
         }
      }
      stage('Run Tests') {
         steps {
         // Use bat for Windows instead of sh
         bat 'docker exec azure-voting-app-redis pytest /tests/test_sample.py'
         }
         post {
            success {
               echo "Tests passed! :)"
            }
            failure {
               echo "Tests failed :("
            }
         }
      }
      stage('Docker Push') {
        steps {
            script {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker tag azure-voting-app-redis %DOCKER_USER%/jenkins-course:2023
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker push %DOCKER_USER%/jenkins-course:2023
                        docker logout
                    '''
                }
            }
        }
      }
   }
   post{
      always {
         bat 'docker compose down'
      }
   }
}
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
   }
}
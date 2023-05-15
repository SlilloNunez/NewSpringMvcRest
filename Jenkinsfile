pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
            }
        }
        stage('Build') {
            steps {
                bat 'start cmd /c mvn -B package'
            
            }
        }
            
        stage('Test') {
            steps {
                bat 'start cmd /c mvn clean verify'
            
            }
        }
    
    post {
         success {
             slackSend "Build deployed successfully - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
      }
   } 
}

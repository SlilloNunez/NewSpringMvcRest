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
    }
    
    post {
       always {
          junit(
        allowEmptyResults: true,
        testResults: '*/test-reports/.xml'
      )
      }
   } 
}

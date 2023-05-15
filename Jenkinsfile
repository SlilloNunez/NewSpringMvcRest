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
        success {
           slackSend channel: '#fundamentos-de-devops', color: '#6EAB00', message: 'Funcionó ', teamDomain: 'sustantivagrupo', tokenCredentialId: 'fg5Q9ww12kqOpcgRHmsGotbH', username: 'Sebastián Lillo Nuñez'
      }
   } 
}

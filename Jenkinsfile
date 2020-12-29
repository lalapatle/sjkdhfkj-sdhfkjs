pipeline {
  agent any
  stages {   
    stage('Maven Compile'){
        steps{
            echo 'Project compile stage'
            bat label: 'Compilation running', script: '''mvn compile'''
               }
    }
   
    stage('Unit Test') {
       steps {
            echo 'Project Testing stage'
            bat label: 'Test running', script: '''mvn test'''
          
       }
       }
   
   
    
     stage('Jacoco Coverage Report') {
            steps{
                    jacoco()
        }
    }
       
    stage('SonarQube'){
        steps{
                bat label: '', script: '''mvn sonar:sonar \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.login=7aaad228b90325c0daa0ecd15a750e4d5584d94e'''
            }
      stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }        
      
      
       
            stage('Maven Package'){
        steps{
            echo 'Project packaging stage'
            bat label: 'Project packaging', script: '''mvn package'''
        }
    }       
   
  }
}
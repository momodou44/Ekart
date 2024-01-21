pipeline {
    agent any
    
    tools { maven 'maven3'
            jdk 'jdk17'
            
    }

     environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git checkout') {
            steps {
               // git branch: 'main', url: 'https://github.com/momodou44/Ekart.git'
                echo 'passed'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn compile"
                
            }
        }

        stage('Sonar Analysis') {
            steps {
               withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Ekart-CICD \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Ekart-CICD '''
               }
            }
        }
   
        
        
    }
}

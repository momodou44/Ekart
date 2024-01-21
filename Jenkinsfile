pipeline {
    agent any
    
    tools { maven 'maven3'
            jdk 'jdk17'
            
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
   
        
        
    }
}

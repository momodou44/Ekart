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
        stage('Build') {
            steps {
               sh "mvn package -DskipTests=true"
            }
        }

        stage('OWASP Dependency Check') {
            steps {
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'global-maven', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy -DskipTests=true"
        }
            }
        }

       
   
        
        
    }
}

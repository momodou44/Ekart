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

        stage('Docker Build & tag Docker image') {
            steps {
                script {
                    withDockerRegistry(credentialsId:'docker-cred', toolName:'docker'){
                        sh " docker build -t mamadoudev/ekart:latest -f docker/Dockerfile ."
                        }
                    }
                }
        }

        stage('trivy scanner') {
            steps {
               sh "trivy  image mamadoudev/ekart:latest > trivy-report.txt "
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId:'docker-cred', toolName:'docker'){
                        sh " docker push -t mamadoudev/ekart:latest"
                        }
                    }
                }
        }

        stage('kubernetes Deploy'){
            steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s-token', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.90.113:6443') {
                    sh "kubectl apply -f deploymentservice.yml -n webapps"
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}

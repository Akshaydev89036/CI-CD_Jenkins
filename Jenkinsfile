pipeline{
    agent any
    tools{
        maven 'maven-3.9'
    }
     stages{
                stage('Git checkout'){
                    steps{
                        git url:'https://github.com/Akshaydev89036/simple-java-maven-app.git',branch:'main'
                    }
                }
                 stage('Build Maven Application '){
                    steps{
                        sh 'mvn clean package'
                    }
                 }
                  stage('Generate sonarqube-analysis'){
                    steps{
                         withSonarQubeEnv(installationName: 'sonarqube-8', credentialsId: 'jenkins-sonar-token'){
                            sh 'mvn sonar:sonar'
                         }
                    }
                 }
                 
                 stage('Upload jar file to nexus')
                 {
                  steps{
                        
                          nexusArtifactUploader artifacts: [
                            [
                                artifactId: 'my-app',
                                classifier: '',
                                 file: 'target/my-app-0.1.jar', 
                                 type: 'jar'
                            ]
                            ], 
                            credentialsId: 'nexus-credentials', 
                            groupId: 'com.mycompany.app', 
                            nexusUrl: '44.213.4.68:8081',
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'Akshay-app', 
                            version: '0.1'
                          }
                      } 
                      
                     
                      stage('Deploy jar file to tomcat'){
                    steps{
                       sshagent(['tomcat-credentials']) {
                           sh """
                           scp -o StrictHostKeyChecking=no target/*.jar ubuntu@54.205.176.10:/opt/tomcat-9/webapps
                           ssh -o StrictHostKeyChecking=no ubuntu@54.205.176.10 /opt/tomcat-9/bin/shutdown.sh
                           ssh -o StrictHostKeyChecking=no ubuntu@54.205.176.10 /opt/tomcat-9/bin/startup.sh
                          
                           """
                           
                             }
                        
                        
                    }
                 } 
                      
                      
                      
                      
                      
            
    }
}

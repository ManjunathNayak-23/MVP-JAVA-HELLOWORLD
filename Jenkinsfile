pipeline {
    agent any
 
    stages {
       
        stage('Build') {
            steps {
                
                script {
                     sh 'mvn clean install -DskipTests'
                }
            }
        }
        stage('Unit Test') {
            steps {
                
                script {
                   
                    sh 'mvn test'
                   
                }
            }
        }
        stage('Integration Test') {
            steps {
                
                script {
                   
                   sh 'mvn verify -DskipUnitTests'
                   
                }
            }
        }
        stage('Sonarqube') {
                      environment {
             scannerHome = tool 'Sonar-scanner'
          }
            steps {
         


                script {
                    withSonarQubeEnv(credentialsId: 'sonarcred', installationName: 'Sonar') {
                  sh """${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=MvpKey \
                                -Dsonar.projectName=MvpProject \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src\
                                -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                                -Dsonar.junit.reportsPath=target/surefire-reports/ \
                                 -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                                 -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml"""
                   
                }
                }
            }
        }
        stage('Push to Nexus'){
            steps{
                script{
                
                    
                    sh "curl -v -u admin:admin --upload-file spring-boot-hello-world-1.0.jar http://34.42.7.89:8081/repository/MVP-DEMO/spring-boot-hello-world-1.0.${env.BUILD_ID}.jar"
                    
                    
                }
                
                
                
                
            }
            
            
        }
        
        
        
    }
}
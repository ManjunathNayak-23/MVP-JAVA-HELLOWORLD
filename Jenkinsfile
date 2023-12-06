pipeline {
    agent any
 
    stages {
       
        stage('Build') {
            steps {
                
                script {
                    sh 'whoami'
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
                                -Dsonar.java.binaries=target/test-classes/com/mkyong \
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
                
                    
                    sh "curl -v -u admin:admin --upload-file target/spring-boot-hello-world-1.0.jar http://34.42.7.89:8081/repository/MVP-DEMO/spring-boot-hello-world-1.0.${env.BUILD_ID}.jar"
                    
                    
                }
                
                
                
                
            }
            
            
        }
        stage('Download artifact'){
            steps{
                script{
            
              sh '''
              cd /opt 
              sudo rm -rf javaapp.jar
              curl -v -o javaapp.jar -u admin:admin http://34.42.7.89:8081/repository/MVP-DEMO/spring-boot-hello-world-1.0.${env.BUILD_ID}
              ''' 
                }
                                
            }


        }
        stage('Start the application'){
             steps{
                script{
                    sh "cd /opt/ && java -jar javapp.jar"
                    echo "started....."
                


                }
             }

        

        }
        
        
        
    }
}

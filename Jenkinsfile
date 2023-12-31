pipeline {
  agent any
  environment {
    NEXUSURL = 'http://172.20.100.248:8081'
    NEXUSREPOID = 'MVP-DEMO'
    NEXUSUSERNAME = 'admin'
    NEXUSPASSWORD = 'admin123'
    MVPKEY='MvpKey'
    MVPPROJECTNAME='MvpProject'

  }

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
            sh """
            ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=${MVPKEY} \
                                -Dsonar.projectName=${MVPPROJECTNAME} \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src\
                                -Dsonar.java.binaries=target/test-classes/com/mkyong \
                                -Dsonar.junit.reportsPath=target/surefire-reports/ \
                                 -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                                 -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                                 """

          }
        }
      }
    }
    stage('Push to Nexus') {
      steps {
        script {

          sh "sudo curl -v -u ${NEXUSUSERNAME}:${NEXUSPASSWORD} --upload-file target/spring-boot-hello-world-1.0.jar ${NEXUSURL}/repository/${NEXUSREPOID}/spring-boot-hello-world-1.0.${env.BUILD_ID}.jar"

        }

      }

    }

    stage('Download artifact and remove old artifact') {
      steps {
        script {

          sh " cd /opt  && sudo rm -rf javaapp.jar && sudo curl -v -o javaapp.jar -u ${NEXUSUSERNAME}:${NEXUSPASSWORD} ${NEXUSURL}/repository/${NEXUSREPOID}/spring-boot-hello-world-1.0.${env.BUILD_ID}.jar"
        }

      }

    }
    stage('Start the application') {
      steps {
        script {
          sh "sudo systemctl restart app.service"
          echo "started....."

        }
      }

    }

  }
}

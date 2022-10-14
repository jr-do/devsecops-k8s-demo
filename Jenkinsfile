pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' // test comment
            }
        }

       stage('Unit Test - JUnit and Jacoco') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                JUnit 'target/surefire-reports/*.xml'
                Jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }   
    }
}
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
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        } 
      stage('Docker Build and Push to docker-hub') {
        steps {
          sh 'printenv'
          sh 'docker build -t abonanno/numeric-app:""$GIT_COMMIT"" . '
          sh 'docker push abonanno/numeric-app:""$GIT_COMMIT""'
        }
      }  
    }
}
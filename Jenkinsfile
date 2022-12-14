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
      stage('Mutations test - PIT') {
            steps {
              sh "mvn org.pitest:pitest-maven:mutationCoverage"
            }
            post {
              always {
                pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
              }
            }
        } 
      stage('Docker Build and Push to docker-hub') {
        steps {
          withDockerRegistry([credentialsId: "docker-hub", url: ""]){
            sh 'printenv'
            sh 'docker build -t abonanno/numeric-app:""$GIT_COMMIT"" . '
            sh 'docker push abonanno/numeric-app:""$GIT_COMMIT""'
          }
        }
      }
    stage('Kubernetes Deployment - DEV') {
        steps {
          withKubeConfig([credentialsId: "kubeconfig"]){
            sh "sed -i 's#replace#abonanno/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
          }
        }
      }  
    }
}
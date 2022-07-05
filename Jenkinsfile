pipeline {
  agent any

  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }
    
    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
    } 
 }
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t lakshit45/fchgrcrgb:""$GIT_COMMIT"" .'
          sh 'docker push lakshit45/fchgrcrgb:""$GIT_COMMIT""'
         }
       }
    }
  }
     
  
  post {
     always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }


// 

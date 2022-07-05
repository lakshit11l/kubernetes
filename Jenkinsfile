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
 
    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t lakshit45/fchgrcrgb:""$GIT_COMMIT"" .'
          sh 'docker push lakshit45/fchgrcrgb:""$GIT_COMMIT""'
         }
       }
    }
    stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh "sed -i 's#replace#lakshit45/fchgrcrgb:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }
    stage('SonarQube - SAST') {
      steps {
        sh "mvn sonar:sonar -Dsonar.projectKey=lakshit  -Dsonar.host.url=http://20.239.91.109:9000  -Dsonar.login=d0e3f78cf5f528d075a637754115442c8862224e"
      }
    }
        
    stage('Vulnerability Scan - Docker ') {
      steps {
        sh "mvn dependency-check:check"
      }
    }
  }
     
  
  post {
     always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
          dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
        }
      }
    }


// 
//

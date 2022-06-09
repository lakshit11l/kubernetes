pipeline {
  agent any
  
  stages {
      stage('Build Artifact') {
            steps {
                   sh "mvn clean package -DskipTests=true"
                   archive 'target/*.jar' //so that they can be downloaded later
                }
            }
      stage('Kubernetes Deployment - DEV') {
        steps {
           withKubeConfig([credentialsId: 'kubeconfig']) {
              // sh "sed -i 's#replace#siddharth67/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
               sh "kubectl apply -f k8s_deployment_service.yaml"
          }
        }
      }
    }
 }

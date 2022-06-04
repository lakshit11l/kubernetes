pipeline {
  agent any
  

  stages {
      stage('Build Artifact') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {   
                   sh "kubectl get pods"
                   sh "mvn clean package -DskipTests=true"
                   archive 'target/*.jar' //so that they can be downloaded later
               }
           }
        }
   }
}


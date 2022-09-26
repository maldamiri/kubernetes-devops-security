pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   
        
      stage('run unit test') {
            steps {
              sh "mvn test" 
            }
            post{
            	always {
            	   junit 'target/surefire-reports/*.xml'
            	   jacoco execPattern: 'target/jacoco.exec'
            	   }  
            	 }
        } 
        
        
      stage('Build Docker') {
            steps {
            withDockerRegistry([credentialsId:"dockerlogin",url:""]){
              sh 'printenv'
              sh 'docker build -t maldamiri/numaric-app:""$GIT_COMMIT"" .'
              sh 'docker push maldamiri/numaric-app:""$GIT_COMMIT""'
              }
            }
        }  
        
     stage('kube deployment') {
            steps {
            withKubeConfig([credentialsId:"kubeconfig"]){
              sh "sed -i 's#replace#maldamiri/numaric-app:$GIT_COMMIT#g' k8s_deployment_service.yaml"
              sh "kubectl apply -f k8s_deployment_service.yaml"
              }
            }
        }  
        
        
        
    }
}
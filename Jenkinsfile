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
              sh 'printenv'
              sh 'docker build -t maldamiri/numaric-app:""$GIT_COMMIT"" .'
              sh 'docker push maldamiri/numaric-app:""$GIT_COMMIT""'
            }
        }  
        
    }
}
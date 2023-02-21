pipeline {
  environment {
    VERSION = "${env.BUILD_ID}"
    dockerimagename = "girishh222/girish5"
    registry = "https://hub.docker.com/"
    registryCredential = 'girishh222/girish5'
    dockerImage = ""	  
	  
    }
	tools{
        maven 'maven3.9.0'
    }
  agent {
  label 'slave01'
  }
  stages{
    stage ('Checkout SCM') {
          steps {
			 git branch: 'main', url: 'https://github.com/GIRISHH222/skydevops-maven-repo.git'
            }
        }    
	
	stage ('Build Package') {

            steps {
               sh 'mvn clean package'
            }
        }
	stage('SonarQube analysis') {
        steps{
          withSonarQubeEnv('Sonar-server') { 
             sh "mvn sonar:sonar"
            }
          }
		}

    stage('Building image') {
      steps{
        script {
         
          dockerImage = docker.build dockerimagename
       }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
              dockerImage.push("${env.BUILD_NUMBER}")
  //          dockerImage.push("latest")
             
          }
        }
      }
    }
  }
}

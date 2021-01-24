pipeline {
  environment {
	registryWeb = "sofienemarmouri/projetynov-web"
	registryApplicants = "sofienemarmouri/projetynov-applicants"
	registryJob = "sofienemarmouri/projetynov-job"
	registryIdentity = "sofienemarmouri/projetynov-identity"
	registrySql = "sofienemarmouri/projetynov-sql"
    registryCredential = 'dockerhub'
	dockerImageWeb = ''
	dockerImageApi = ''
	dockerImageJob = ''
	dockerImageIdentity = ''
	dockerImageSql = ''
  }
  agent any

  stages {
    stage('Git checkout') {
      steps {
         checkout scm
      }
    }
    
    stage('Building image') {
      steps{
          dir ( 'appscore'){
          script {
	   sh "docker pull redis"
	   sh "docker pull rabbitmq"
           sh "pwd;ls -la"
		  dockerImageWeb = docker.build(registryWeb,"-f Web/Dockerfile .")
		  dockerImageApi = docker.build(registryApplicants,"-f Services/Applicants.Api/Dockerfile .")
		  dockerImageJob = docker.build(registryJob,"-f Services/Jobs.Api/Dockerfile .")
		  dockerImageIdentity = docker.build(registryIdentity,"-f Services/Identity.Api/Dockerfile .")
	  dir ('appscore/Database'){ 
	  script {
	   sh "pwd;ls -la"
		  dockerImageSql = docker.build(registrySql,"-f Dockerfile .")
	  }	  
	 }	  
        }
      }}
    }
    stage('Publish Image ') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImageWeb.push("$BUILD_NUMBER")
            dockerImageWeb.push("latest")
	    dockerImageApi.push("$BUILD_NUMBER")
            dockerImageApi.push("latest")
	    dockerImageJob.push("$BUILD_NUMBER")
            dockerImageJob.push("latest")
	    dockerImageIdentity.push("$BUILD_NUMBER")
            dockerImageIdentity.push("latest")
	    dockerImageSql.push("$BUILD_NUMBER")
            dockerImageSql.push("latest")
	   sh "docker tag redis user.data"
	   sh "docker tag rabbitmq:latest sofienemarmouri/projetynov-rabbitmq:rabbitmq"
	   sh "docker tag user.data:latest sofienemarmouri/projetynov-userdata:user.data"
	   sh "docker push sofienemarmouri/projetynov-rabbitmq:rabbitmq"
	   sh "docker push sofienemarmouri/projetynov-userdata:user.data"
          }
           echo "trying to push Docker Build to DockerHub"
        }
      }
    }

    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registryWeb:$BUILD_NUMBER"
	sh "docker rmi $registryApplicants:$BUILD_NUMBER"
	sh "docker rmi $registryJob:$BUILD_NUMBER"
	sh "docker rmi $registryIdentity:$BUILD_NUMBER"
	sh "docker rmi $registrySql:$BUILD_NUMBER"
      }
    }
}
  
} 

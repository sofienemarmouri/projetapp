pipeline {
  environment {
	registryWeb = "sofienemarmouri/projetynov-web"
	registryApplicants = "sofienemarmouri/projetynov-applicants"
	registryJob = "sofienemarmouri/projetynov-job"
	registryJob = "sofienemarmouri/projetynov-identity"
    registryCredential = 'dockerhub'
	dockerImageWeb = ''
	dockerImageApi = ''
	dockerImageJob = ''
	dockerImageIdentity = ''
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
           sh "pwd;ls -la"
		  dockerImageWeb = docker.build(registryWeb,"-f Web/Dockerfile .")
		  dockerImageApi = docker.build(registryApplicants,"-f Services/Applicants.Api/Dockerfile .")
		  dockerImageJob = docker.build(registryJob,"-f Services/Jobs.Api/Dockerfile .")
		  dockerImageIdentity = docker.build(registryIdentity,"-f Services/Identity.Api/Dockerfile .")
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
	    dockerImageJob.push("$BUILD_NUMBER")
            dockerImageJob.push("latest")
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
      }
    }
}
  
} 

pipeline {
  environment {
	registryWeb = "sofienemarmouri/projetynov-web"
	registryApplicants = "sofienemarmouri/projetynov-applicants"
	registryJob = "sofienemarmouri/projetynov-job"
	registryIdentity = "sofienemarmouri/projetynov-identity"
	registryRedis = "sofienemarmouri/projetynov-redis"
    registryCredential = 'dockerhub'
	dockerImageWeb = ''
	dockerImageApi = ''
	dockerImageJob = ''
	dockerImageIdentity = ''
	dockerImageRedis = ''
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
		  dockerImageRedis = docker.image("redis")
		  dockerImageRedis.pull()
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
	    dockerImageRedis.push("$BUILD_NUMBER")
            dockerImageRedis.push("latest")
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
	sh "docker rmi $registryRedis:$BUILD_NUMBER"
      }
    }
}
  
} 

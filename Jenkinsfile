node{
	stage("Git clone"){
		git credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/surjeetkm/sample-istio-services.git'
	}
	stage("Maven clean build artifact"){	
		def mavenHome= tool name: "Maven", type: "maven"
		def command= "${mavenHome}/bin/mvn"
		sh "${command} clean install"
	}
	stage("Run Junit and Integration Test cases"){}
	stage('Caller-service image') {
   	 	dir ('caller-service') {
    			app=docker.build("microservices-2020/caller-service:1.0")
    			docker.withRegistry('https://eu.gcr.io', 'gcr:myregistry') {
 	 			app.push("${env.BUILD_NUMBER}")
 	 			
        		}
  		}
  	}
  	
  	stage("callme image"){
       		dir ('callme-service') {
    			app=docker.build("microservices-2020/callme-service:1.0")
			app=docker.build("microservices-2020/callme-service:2.0")
    			docker.withRegistry('https://eu.gcr.io', 'gcr:myregistry') {
 	 			app.push("${env.BUILD_NUMBER}")
 	 			
        		}
        	}
     }  
}

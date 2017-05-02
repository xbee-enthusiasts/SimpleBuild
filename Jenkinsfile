node {
   
   	stage('Checkout') {
    		checkout scm
	}

   	stage('Build') {
		slackSend "$JOB_NAME: Build begun."
		def mvnHome = tool 'M3'
  		sh "${mvnHome}/bin/mvn -B clean package"
		slackSend "$JOB_NAME: Build has finished."
	}

   	stage('Archive') {
		fingerprint '*.bar'
		archiveArtifacts '*.bar'
	}

}

node {
   

   	stage('Build') {
		slackSend "$JOB_NAME: Build begun."
		def mvnHome = tool 'M3'
  		sh "${mvnHome}/bin/mvn -B clean package"
		slackSend "$JOB_NAME: Build finished."
	}

   	stage('Archive') {
		fingerprint '*.bar'
		archiveArtifacts '*.bar'
	}

}

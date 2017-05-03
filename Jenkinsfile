node {


	stage('Checkout') {
		checkout scm
	}
   

   	stage('Build') {
		slackSend "$JOB_NAME: Build begun."
		def mvnHome = tool 'M3'
  		sh "${mvnHome}/bin/mvn -B clean package"
	}

   	stage('Archive') {
		archiveArtifacts allowEmptyArchive: true, artifacts: '*.jar', fingerprint: true, onlyIfSuccessful: true
	}

}

node {

	/*
   	stage('Checkout') {
		checkout([$class: 'GitSCM'
                	//, branches: [[name: '*/master']]
                	, doGenerateSubmoduleConfigurations: false
                	, extensions: [[$class: 'CleanCheckout']
                	, [$class: 'PerBuildTag']]
                	, submoduleCfg: []
               		// , userRemoteConfigs: [[url: 'git@github.sherwin.com:TAG-IT-Commerce-Dev/SimpleBuild.git']]
		]
            )
	}
	*/

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

node {

   	stage('Checkout') {
		checkout([$class: 'GitSCM'
                //, branches: [[name: '*/master']]
                , doGenerateSubmoduleConfigurations: false
                , extensions: [[$class: 'CleanCheckout']
                , [$class: 'PerBuildTag']]
                , submoduleCfg: []
                , userRemoteConfigs: [[url: 'git@github.sherwin.com:TAG-IT-Commerce-Dev/SimpleBuild.git']]]
            )
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

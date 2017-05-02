node {
   
   	stage('Checkout') {
		checkout([$class: 'GitSCM'
                , branches: [[name: "*/release/${REL_VER}"]]
                , doGenerateSubmoduleConfigurations: false
                , extensions: [[$class: 'CleanCheckout']
                , [$class: 'PerBuildTag']]
                , submoduleCfg: []
                , userRemoteConfigs: [[url: 'git@github.sherwin.com:TAG-IT-Commerce-Dev/Smis2App.git']]]
            )
	}

   	stage('Build') {
		slackSend "$JOB_NAME: IIB Build begun."
		parallel buildBackend: {
			wrap([$class: 'Xvfb']) {
   				sh """
                  			./iib-build.sh
                  			# Stand-in for 
            			"""
			}
		}, buildFrontend: {
			def mvnHome = tool 'M3'
  			sh "${mvnHome}/bin/mvn -B clean package"
		}
		slackSend "$JOB_NAME: Build finished."
/*
		withCredentials([usernamePassword(credentialsId: 'tag-smis', passwordVariable: 'gitpass', usernameVariable: 'gituser')]) {
			sh 'git push --tags origin'
		}
*/
	}

   	stage('Archive') {
		fingerprint '*.bar'
		archiveArtifacts '*.bar'
	}

	stage('Deploy') {
		parallel devDeployBackend: {
			withCredentials([usernameColonPassword(credentialsId: 'iib_dev_deploy_user', variable: 'iibcreds')]) {
				withEnv(['iibhost=dev']) {
					sh "./iib-deploy.sh Smis2BackendEG-${REL_VER}.dev.bar"
					slackSend "$JOB_NAME: IIB Deployed to $iibhost"
                		}
			}
		} , qaaDeployBackend: {
			withCredentials([usernameColonPassword(credentialsId: 'iib_dev_deploy_user', variable: 'iibcreds')]) {
				withEnv(['iibhost=qaa']) {
					sh "./iib-deploy.sh Smis2BackendEG-${REL_VER}.qa.bar"
					slackSend "$JOB_NAME: IIB Deployed to $iibhost"
				}
			}
		} , qabDeployBackend: {
			withCredentials([usernameColonPassword(credentialsId: 'iib_dev_deploy_user', variable: 'iibcreds')]) {
				withEnv(['iibhost=qab']) {
					sh "./iib-deploy.sh Smis2BackendEG-${REL_VER}.qa.bar"
					slackSend "$JOB_NAME: IIB Deployed to $iibhost"
				}
			}
		} , qacDeployBackend: {
			withCredentials([usernameColonPassword(credentialsId: 'iib_dev_deploy_user', variable: 'iibcreds')]) {
				withEnv(['iibhost=qac']) {
					sh "./iib-deploy.sh Smis2BackendEG-${REL_VER}.qa.bar"
					slackSend "$JOB_NAME: IIB Deployed to $iibhost"
				}
		}
		} , qadDeployBackend: {
			withCredentials([usernameColonPassword(credentialsId: 'iib_dev_deploy_user', variable: 'iibcreds')]) {
				withEnv(['iibhost=qad']) {
					sh "./iib-deploy.sh Smis2BackendEG-${REL_VER}.qa.bar"
					slackSend "$JOB_NAME: IIB Deployed to $iibhost"
				}
			}
		}
	}
}

#!groovy

node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def DEV_HUB = env.SF_USERNAME
    def CONNECTED_APP_CONSUMER_KEY = env.SF_CONSUMER_KEY

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

	stage('Test sfdx') {
		rc = sh returnStatus: true, script: "${toolbelt}/sfdx --version"
		println rc
	}    
}
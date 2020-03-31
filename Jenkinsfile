#!groovy

node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def DEV_HUB = env.SF_USERNAME
    def CONNECTED_APP_CONSUMER_KEY = env.SF_CONSUMER_KEY
    def JWT_KEY_FILE = env.SERVER_KEY_CREDENTIALS_ID

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

	stage('Test sfdx') {
		rc = sh returnStatus: true, script: "${toolbelt}/sfdx --version"
		println rc
	}

    withCredentials([file(credentialsId: JWT_KEY_FILE, variable: 'jwt_key_file')]) {
        stage('Authorize to Salesforce') {
		    println CONNECTED_APP_CONSUMER_KEY
		    println DEV_HUB
		    println jwt_key_file

			rc = command "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${jwt_key_file} --username ${DEV_HUB} "
		    if (rc != 0) {
			    error 'Salesforce org authorization failed.'
		    }
		}
    }
}
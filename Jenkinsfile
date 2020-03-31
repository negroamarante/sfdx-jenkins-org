#!groovy

node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def DEV_HUB = env.SF_USERNAME
    def CONNECTED_APP_CONSUMER_KEY = env.SF_CONSUMER_KEY
    def SERVER_KEY_CREDENTIALS_ID = env.SERVER_KEY_CREDENTIALS_ID

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

	stage('Test sfdx') {
		rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:org:list --all"
	}

    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'jwt_key_file')]) {
        stage('Authorize to Salesforce') {

			rc = command "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${SERVER_KEY_CREDENTIALS_ID} --username ${DEV_HUB} "
		    println rc
		    if (rc != 0) {
			    error 'Salesforce org authorization failed.'
		    }
		}
    }
}
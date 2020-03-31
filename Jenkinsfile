#!groovy

node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    // def SFDC_USERNAME

    def DEV_HUB = env.SF_USERNAME
    def CONNECTED_APP_CONSUMER_KEY = env.SF_CONSUMER_KEY
    def SERVER_KEY_CREDENTIALS_ID = env.SERVER_KEY_CREDENTIALS_ID

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

	stage('sfdx version') {
		rc = sh returnStatus: true, script: "${toolbelt}/sfdx --version"
	}

    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'jwt_key_file')]) {
        stage('Authorize to Salesforce') {
            rc = sh returnStatus: true, script: "${toolbelt}/sfdx --version"

			rc = script "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --jwtkeyfile ${jwt_key_file} --username ${DEV_HUB} "
		    if (rc != 0) {
			    error 'Salesforce org authorization failed.'
		    }
		}

        stage('Create Scratch Org') {

            // need to pull out assigned username
            rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx force:org:create --definitionfile config/project-scratch-def.json --json --setdefaultusername"
            printf rmsg
            // def jsonSlurper = new JsonSlurperClassic()
            // def robj = jsonSlurper.parseText(rmsg)
            // if (robj.status != 0) { error 'org creation failed: ' + robj.message }
            // SFDC_USERNAME=robj.result.username
            // robj = null
        }

        stage('Push To Scratch Org') {
            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:source:push "
            // rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:source:push --targetusername ${SFDC_USERNAME}"
            if (rc != 0) {
                error 'push failed'
            }
        }
    }
}
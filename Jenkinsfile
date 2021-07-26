node {

    def SF_CONSUMER_KEY=env.clientId
    def SF_USERNAME=env.userName
    def SERVER_KEY_CREDENTIALS_ID=env.SERVER_KEY_CREDENTIALS_ID
    def DEPLOYDIR='src'
    def TEST_LEVEL='RunLocalTests'
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://test.salesforce.com"


    def toolbelt = tool 'toolbelt'
    stage('checking'){
        echo 'working'
    }
	withEnv(["HOME=${env.WORKSPACE}"]) {
		withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) {
			stage('Authorize to Salesforce') {
				rc = command "${toolbelt}/sfdx auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_USERNAME} --setalias UAT"
				if (rc != 0) {
				error 'Salesforce org authorization failed.'
				}
			}
		}
	}
}
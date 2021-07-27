node {
    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
    def SF_USERNAME=env.SF_USERNAME
    def SERVER_KEY_CREDENTIALS_ID=env.server_key_credentials_id
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
	def TEST_LEVEL='%Testlevel%'
	def DELTA='DeltaChanges'
	def DEPLOYDIR='toDeploy'
	def APIVERSION='51.0'

    stage('Clean Workspace') {
        try {
            deleteDir()
			currentBuild.displayName = "SFDX DEMO PIPELINE ${BUILD_NUMBER}"
        }
        catch (Exception e) {
            println('Unable to Clean WorkSpace.')
        }
    }

    stage('checkout source'){
	     checkout scm
    }

	stage('print server key'){
	    echo SERVER_KEY_CREDENTIALS_ID;
	    echo SF_CONSUMER_KEY;
	    echo SF_USERNAME;
	}

 	withEnv(["HOME=${env.WORKSPACE}"]){
	    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]){
			stage('Authorize to Salesforce'){
		        sh '''sfdx auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_USERNAME} --setalias SFDX'''
		}
	}
}

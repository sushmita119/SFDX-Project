#!groovy

node 
{

    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
    def SF_USERNAME=env.SF_USERNAME
    def SERVER_KEY_CREDENTIALS_ID=env.server_key_credentials_id
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
	def TEST_LEVEL='%Testlevel%'
	def DELTA='DeltaChanges'
	def DEPLOYDIR='toDeploy'
	def APIVERSION='51.0'
    def toolbelt = tool 'toolbelt'
	

	


    stage('Clean Workspace') 
	{
        try 
		{
            deleteDir()
			currentBuild.displayName = "SFDX DEMO PIPELINE ${BUILD_NUMBER}"
        }
        catch (Exception e) 
		{
            println('Unable to Clean WorkSpace.')
        }
    }
    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') 
	{
        checkout scm
		def branchOriginName = bat (label: 'Branch name', script: '@git name-rev --name-only HEAD', returnStdout: true).trim() as String   
        branchName = branchOriginName.replaceAll('remotes/origin/','').split('~')[0]
        println branchName
    
    }
	stage('print server key'){
	echo SERVER_KEY_CREDENTIALS_ID;
		}

    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------

 	withEnv(["HOME=${env.WORKSPACE}"]) 
	{	
	
	    withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) 
		{
			// -------------------------------------------------------------------------
			// Authenticate to Salesforce using the server key.
			// -------------------------------------------------------------------------

			stage('Authorize to Salesforce') 
			{	 bat """
				rc = command "${toolbelt}/sfdx auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${server_key_file} --username ${SF_USERNAME} --setalias SFDX"
					'echo %rc%'
			}
		}
	}
}

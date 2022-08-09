pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/eitservicesindiaprivatelimited/"
			UIPATH_ORCH_LOGICAL_NAME = "eitservicesindiaprivatelimited"
			UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
			UIPATH_ORCH_FOLDER_NAME = "Default"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
			// Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
                UiPathDeploy (
                packagePath: "Output\\${env.BUILD_NUMBER}",
                orchestratorAddress: "${UIPATH_ORCH_URL}",
                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                environments: 'DEV',
                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
                credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
				traceLevel: 'None',
				entryPointPaths: 'Main.xaml'
	

	        )
	            }
	        }
	    }
		
	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	}

#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }else{
                 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			}else{
				println "toolbelt-AI:"
				println "\"${toolbelt}\" force:source:deploy --sourcepath manifest/. -u ${HUB_ORG}"
			   //rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			   //rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy --sourcepath manifest/. -u ${HUB_ORG}"
				//rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy -m ApexClass"
                //rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:retrieve --manifest c:\projects_sfdx\jenkins2\manifest\package.xml"
                //rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:retrieve --manifest c:\projects_sfdx\jenkins2\manifest\package.xml"
                //rmsg = bat returnStdout: true, script: "sfdx force:source:deploy --sourcepath c:\projects_sfdx\jenkins2\force-app\main\default\classes\AccountController.cls"
				//println "\"${toolbelt}\" force:source:deploy --sourcepath force-app\main\default\classes\AccountController.cls"
				//rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy --sourcepath force-app\main\default\classes\AccountController.cls"
				//$ sfdx force:source:deploy -m ApexClass
				
				println "\"${toolbelt}\" force:source:deploy --manifest manifest/. -u ${HUB_ORG}"
				//rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy --manifest ./manifest/package.xml -u ${HUB_ORG}"
				rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
				
			println('AI-1')	
			}
			  
            //printf rmsg
            println('Hello from a Job DSL script!')
            //println(rmsg)
        }
    }
}

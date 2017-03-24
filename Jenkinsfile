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

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Create Scratch Org') {



            rc = sh returnStatus: true, script: "${toolbelt}/sfdx _ force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile C:\'\\'Anbu\'\\'Innovation\'\\'SalesforceDX\'\\'Pilot\'\\'server.key  --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            
            if (rc != 0) { error 'hub org authorization failed' }
        }

        stage('Push To Test Org') {
        Print "In Push to Test Org"
            rc = sh returnStatus: true, script: "${toolbelt}/sfdx _ force:source:push --targetusername scratchorg1490349215952@anbu.com"
            if (rc != 0) {
                error 'push failed'
            }
            // assign permset
            rc = sh returnStatus: true, script: "${toolbelt}/sfdx _ force:user:permset:assign --targetusername scratchorg1490349215952@anbu.com --permsetname DreamHouse"
            if (rc != 0) {
                error 'permset:assign failed'
            }
        }

        }
    }


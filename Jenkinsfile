#!/usr/env/groovy
node() {

    properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5', quietPeriod: '60']],disableConcurrentBuilds()])
    
    try {

        if (env.BRANCH_NAME.matches(/^(?:v)?(\d+\.)?(\d+\.)?(\*|\d+)(?:SR\d+)?$/)) {
            RELEASE_VERSION = env.BRANCH_NAME
        } else {
            RELEASE_VERSION = env.BRANCH_NAME + "-SNAPSHOT"
        }

        stage('checkout') {
            //deleteDir()
            checkout scm
        }

       

        currentBuild.result = "SUCCESS"

    } catch (err) {
        currentBuild.result = "FAILURE"
        step([$class: 'JUnitResultArchiver', testResults:
                '**/target/**/TEST-*.xml'])

        throw err
    }
    finally {
       
           office365ConnectorSend webhookUrl: "https://spiglobal365.webhook.office.com/webhookb2/0ea2c16a-c529-4ef8-ba02-25b69c6d7c9d@2ed42162-0b0a-4972-b5e1-1c6006bd8df4/JenkinsCI/8d97f93fee2c4213908ae72b1ca689f3/cb35c1b5-58a6-4c55-a0bb-485106272224",
                factDefinitions: [[name: "fact1", template: "content of fact1"],
                                  [name: "fact2", template: "content of fact2"]]
         }

}

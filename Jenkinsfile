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
       
           office365ConnectorSend webhookUrl: "https://outlook.office.com/webhook/4c165a4a-3f93-42ff-9ff8-77386e4412f8@2ed42162-0b0a-4972-b5e1-1c6006bd8df4/JenkinsCI/3ead401f5fa047c690529054cef97049/cb35c1b5-58a6-4c55-a0bb-485106272224",
                factDefinitions: [[name: "fact1", template: "content of fact1"],
                                  [name: "fact2", template: "content of fact2"]]
         }

}

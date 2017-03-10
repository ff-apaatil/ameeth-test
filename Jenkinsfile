#!/usr/bin/env groovy


if (env.BRANCH_NAME == 'master') {
  keepDays = 60
}
else {
  keepDays = 7
}

properties([
    buildDiscarder(
      logRotator(
        artifactDaysToKeepStr: '',
        artifactNumToKeepStr: '',
        daysToKeepStr: "${keepDays}",
        numToKeepStr: '30'
        )
    )
])

timeout(time: 60, unit: 'MINUTES'){
 node() {
    wrap([$class: 'TimestamperBuildWrapper']){
      string currentBuild.result;
      //string mailerlist = 'rsood@firstfuel.com';
      currentBuild.result = 'SUCCESS'

      stage('Deploy Common'){
        try{
          sh 'mvn clean deploy'
          currentBuild.result = 'SUCCESS'
        }
        catch(e){
          echo 'Nexus Deployment has failed'
          Notify()
          throw(e)
        }
      }
      

}

        

def Notify() {
    try{
        println currentBuild.result
        if (currentBuild.result == 'SUCCESS'){
          colorcode = '#008000'
        }
        else {
          colorcode = '#FF0000'
        }
      }

    catch(e){
        println currentBuild.result
        throw e
      }

    withCredentials([[$class: 'StringBinding',
    credentialsId: 'SlackIntegrationToken',
    variable: 'SlackIntegrationToken']])
    {
    slackSend (channel: '#jenkins',
    color: "$colorcode",
    message: "Job '${currentBuild.result}' '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

    step([$class: 'Mailer',
    notifyEveryBuild: true,
    //recipients: "${mailerlist}",
    //sendToIndividuals: true])
    recipients: emailextrecipients([[$class: 'CulpritsRecipientProvider'],
                   [$class: 'RequesterRecipientProvider']])])
  }
}

def reports() {
  step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true])
}

def projectIdJson(jsonString){
    def slurper = new groovy.json.JsonSlurper()
    def result = slurper.parseText(jsonString)
    return " "+result[0].id
}

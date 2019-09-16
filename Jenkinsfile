pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    environment {
        CI = 'true'
        appName = 'NodeJs + React'
        appType = 'WEB'
    }
    try{
        stages {
            stage('Build') { 
                steps {
                    slackSend(channel: "pipeline", message: "[${appType}]${appName} - Job Started! :)", sendAsText: true)
                    sh 'npm install' 
                }
            }
            stage('Test'){
                steps{
                    sh './jenkins/scripts/test.sh'
                }
            }
            stage('Deliver') {
                steps {
                    sh './jenkins/scripts/deliver.sh'
                    input message: 'Finished using the web site? (Click "Proceed" to continue)'
                    sh './jenkins/scripts/kill.sh'
                }
            }
            stage('Slark Noti') {
                steps {
                    slackSend(channel: "pipeline", message: "[${appType}]${appName} - Success! :)", sendAsText: true)
                }
            }
        }            
    } catch (e) {
        slackSend(channel: "pipeline",color: "danger", message: "[${appType}]${appName} - Failed! :)", sendAsText: true)
    }
}
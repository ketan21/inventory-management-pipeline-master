pipeline {
  agent any
  tools {nodejs "mynode"}
  options {
    timestamps()
    timeout(time: 15, unit: 'MINUTES')
    withCredentials([
      usernamePassword(credentialsId: 'RMCREDENTIALS',
        passwordVariable: 'IMS_PASSWORD',
        usernameVariable: 'IMS_USER')
    ])
  }


  stages {

    stage ('CheckoutPostmanTest') {
      steps{
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'soapui']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'mhgit', url: 'https://github.com/mvanhNL/soapui.git']]])
      }
    }
    stage('RunMyTest') {
      steps {
        sh 'newman run --reporters junit ls.json'
        junit 'newman/*.xml'

      }
    }
  }

  post {
    success {
      //sh 'curl -v --user $IMS_USER:$IMS_PASSWORD -H "Content-Type: application/json" -X POST --data {\"jobName\":\"$JOB_NAME\",\"buildNumber\":\"$BUILD_NUMBER\",\"pyStatusValue\":\"SUCCESS\",\"pyID\":\"$BuildID\"} "$CallBackURL" '
      echo 'wel goed'
    }
    failure {
      //sh 'curl -v --user $IMS_USER:$IMS_PASSWORD -H "Content-Type: application/json" -X POST --data {\"jobName\":\"$JOB_NAME\",\"buildNumber\":\"$BUILD_NUMBER\",\"pyStatusValue\":\"FAIL\",\"pyID\":\"$BuildID\"} "$CallBackURL" '
      echo 'niet goed'
    }
    always {
      //cleanWs notFailBuild: true, patterns: [[pattern: '***.xml', type: 'INCLUDE']]
      echo 'altijd doen'
    }
  }
}

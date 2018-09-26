pipeline {
  agent any
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
        //sh 'soapui/bin/testrunner.sh -f soapui/tests/firsttest/reports -j soapui/tests/firsttest/firsttest.xml'
        //junit 'soapui/tests/firsttest/reports/*.xml'
        //nodejs('nodejs') {
          // some block
        //}
        newman -c https://www.getpostman.com/collections/457dc4e55a02a5d4c5b1 
        echo 'nu nog niets'
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

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
    stage ('CheckoutSoapUI') {
      steps{
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'soapui']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'mhgit', url: 'https://github.com/mvanhNL/soapui.git']]])
      }
    }
    stage ('CheckJavaVersion') {
      steps {
        sh 'java -version'
      }
    }
    stage('RunMyTest') {
      steps {
        sh 'soapui/bin/testrunner.sh -s soapui/tests/firsttest/firsttest.xml'
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
  }
}

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
    stage ('Check4SoapUI') {
      steps{
      if (fileexists ('bin/testrunner.sh')) {
        echo 'okay, file is there'
      } else {
        echo 'testrunner is not here'
        echo 'do a checkout of a specific git location to copy the soapui stuff to local server'
        sh 'java --version'
      }
    }
    }
    stage ('CheckoutTestScript') {
      steps {
      echo ' do a checkout of the specified script from git to local'
      echo ' the scriptfilename should be given as parameter in the pega job'
      echo 'convention: foldername=scriptname..'
    }
    }
    stage('RunMyTest') {
      steps {

        //sh 'bin/testrunner.sh -s fjhdkg/$scriptname etc etc'

      }
    }
  }

  post {
    success {
      sh 'curl -v --user $IMS_USER:$IMS_PASSWORD -H "Content-Type: application/json" -X POST --data {\"jobName\":\"$JOB_NAME\",\"buildNumber\":\"$BUILD_NUMBER\",\"pyStatusValue\":\"SUCCESS\",\"pyID\":\"$BuildID\"} "$CallBackURL" '
    }
    failure {
      sh 'curl -v --user $IMS_USER:$IMS_PASSWORD -H "Content-Type: application/json" -X POST --data {\"jobName\":\"$JOB_NAME\",\"buildNumber\":\"$BUILD_NUMBER\",\"pyStatusValue\":\"FAIL\",\"pyID\":\"$BuildID\"} "$CallBackURL" '

    }
  }
}

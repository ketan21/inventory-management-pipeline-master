pipeline {
  agent any
  options {
    timestamps()
    timeout(time: 15, unit: 'MINUTES')
    withCredentials([
      usernamePassword(credentialsId: 'imsadmin',
        passwordVariable: 'IMS_PASSWORD',
        usernameVariable: 'IMS_USER')
    ])
  }

  stages {
    stage('RunMyTest') {
      steps {
        echo 'Execute tests'
        withEnv(['TESTRESULTSFILE="TestResult.xml"']) {
          //   sh "./gradlew executePegaUnitTests -PtargetURL=${PEGA_DEV} -PpegaUsername=${IMS_USER} -PpegaPassword=${IMS_PASSWORD} -PtestResultLocation=${WORKSPACE} -PtestResultFile=${TESTRESULTSFILE}"
          // junit "TestResult.xml"
          script {
            if (currentBuild.result != null) {
              error("PegaUNIT tests have failed in Dev.")
            }
          }
        }
      }
    }
  }

  post {
    success {
      sh 'curl --user %RMCREDENTIALS% -H "Content-Type: application/json" -X POST --data "{\"jobName\":\"%JOB_NAME%\",\"buildNumber\":\"%BUILD_NUMBER%\",\"pyStatusValue\":\"SUCCESS\",\"pyID\":\"%BuildID%\"}" "%CallBackURL%" '
      
    }

  }
}

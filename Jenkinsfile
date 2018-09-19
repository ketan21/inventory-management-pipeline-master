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
    stage('RunMyTest') {
      steps {
        echo 'Execute tests'
        echo "${params.param1}"
        echo "${params.CallBackURL}"
        echo "${params.BuildID}"
        sh 'echo $BuildID'
        sh 'echo $JOB_NAME'

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
      sh 'curl --user $IMS_USER:$IMS_PASSWORD -H "Content-Type: application/json" -X POST --data "{\"jobName\":\"$JOB_NAME\",\"buildNumber\":\"$BUILD_NUMBER\",\"pyStatusValue\":\"SUCCESS\",\"pyID\":\"$BuildID\"}" "$CallBackURL" '

    }

  }
}

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
    stage('DoSomething') {
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

      stage('PegaUNITs in QA') {
            steps {
                echo 'Execute tests'

                withEnv(['TESTRESULTSFILE="TestResult.xml"']) {
                //    sh "./gradlew executePegaUnitTests -PtargetURL=${PEGA_QA} -PpegaUsername=${IMS_USER} -PpegaPassword=${IMS_PASSWORD} -PtestResultLocation=${WORKSPACE} -PtestResultFile=${TESTRESULTSFILE}"
                //    junit "TestResult.xml"
                script {
                       if (currentBuild.result != null) {
                           error("PegaUNIT tests have failed in QA.")
                        }
                 }

        }
    }
    }
  }

  post {

    failure {
      mail (
          subject: "${JOB_NAME} ${BUILD_NUMBER} merging branch ${branchName} has failed",
          body: "Your build ${env.BUILD_NUMBER} has failed.  Find details at ${env.RUN_DISPLAY_URL}",
          to: notificationSendToID
      )
    }
  }
}

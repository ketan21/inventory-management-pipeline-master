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

    stage('RunMyTest') {
      steps {
        sh 'curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py'
        sh 'python get-pip.py --user'
        sh '/var/jenkins_home/.local/bin/pip install robot-framework'
        sh 'pybot'
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

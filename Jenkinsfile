pipeline {
  agent any

  stages {
    stage('Install Nginx') {
      steps {
        script {
          def result = salt(
            authtype: 'pam',
            clientInterface: runner(
              function: 'state.orchestrate',
              arguments: 'orch.nginx'
            ),
            credentialsId: 'saltuser-creds',
            saveFile: true,
            servername: 'http://172.31.32.34:8000'
          )
          echo groovy.json.JsonOutput.prettyPrint(groovy.json.JsonOutput.toJson(result))
        }
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}

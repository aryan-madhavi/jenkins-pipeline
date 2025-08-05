pipeline {
  agent any

  stages {
    stage('Run Salt') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: [
              'nginx-jenkins',
              'nginx-start-jenkins',
            ],
            blockbuild: true,
            jobPollTime: 6,
            target: '*',
            targettype: 'glob'
          ),
          credentialsId: 'saltuser-creds',
          saveFile: true,
          servername: 'http://172.31.6.109:8080'
        )

        script {
          def output = readFile "${env.WORKSPACE}/saltOutput.json"
          echo output
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

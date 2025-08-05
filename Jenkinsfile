pipeline {
  agent any

  stages {
    stage('Install Nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx_jenkins',
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
          def json = new groovy.json.JsonSlurper().parseText(output)
          echo groovy.json.JsonOutput.toJson(json)
        }
      }
    }
    stage('Start nginx') {
      steps {
        salt(
          authtype: 'pam',
          clientInterface: local(
            function: 'state.apply',
            arguments: 'nginx_start_jenkins',
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
          def json = new groovy.json.JsonSlurper().parseText(output)
          echo groovy.json.JsonOutput.toJson(json)
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

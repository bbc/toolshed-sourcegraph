pipeline {
      agent any

      options {
          disableConcurrentBuilds()
          buildDiscarder(logRotator(numToKeepStr: '60'))
          timeout(time: 7, unit: 'DAYS')
      }

      stages {
          stage('Deploy') {
              steps {
                  script {
                      def hasYaml = fileExists 'docker-compose.yml'
                      if (hasYaml) {
                          sh "curl -H 'Content-type: text/x-yaml' --cert /etc/pki/tls/certs/client.crt --key /etc/pki/tls/private/client.key --request PUT --data-binary @docker-compose.yml https://planter.toolshed.tools.bbc.co.uk/sourcegraph"
                      } else {
                          sh 'echo "ERROR: Please create a docker-compose.yml file to deploy to Toolshed" && false'
                      }
                  }
              }
          }
      }
  }

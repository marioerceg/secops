pipeline {
  agent any 

  stages {
    stage('Daily Compliance Run') {
      steps{
        echo 'Running a compliance scan with inspec....'
          script{
            def remote = [:]
            remote.name = "controlnode"
            remote.host = "34.79.54.46"
            remote.allowAnyHosts = true

            withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                remote.user = userName
                remote.identityFile = identity
                stage("Placeholder Stage...") {
                  sshCommand remote: remote, sudo: false, command: 'cd /root/secops/ansible && git pull origin'
                  sshCommand remote: remote, sudo: false, command: 'cd /root/secops/ansible && ansible-playbook compliance.yaml'
              }
                stage("Scan with InSpec") {
                  sshCommand remote: remote, sudo: false, command: 'inspec exec --no-distinct-exit /root/linux-baseline/'
              }
            }
          }
       }
    }
  }
}


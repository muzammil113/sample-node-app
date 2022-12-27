pipeline {
    agent any

      stages {
          stage('build') {
              steps {
                  echo 'building the software'
                  sh 'npm install'
              }
          }
          stage('test') {
              steps {
                  echo 'testing the software'
                  sh 'npm test'
              }
          }

          stage('deploy') {
              steps {
                withCredentials([sshUserPrivateKey(credentialsId: "jenkins-ssh", keyFileVariable: 'sshkey')]){
                  echo 'deploying the software'
                  sh ''' #!/bin/bash
                  echo "Creating .ssh"
                  mkdir -p /var/lib/jenkins/.ssh
                  ssh-keyscan 104.248.226.187 >> /var/lib/jenkins/.ssh/authorized_keys
                  

                  rsync -avz --exclude  '.git' --delete -e "ssh -i $sshkey" ./ demo-jenkins-server@104.248.226.187:/app/
           

                  ssh -i $sshkey demo-jenkins-server@104.248.226.187 "sudo systemctl restart nodeapp"
           
              }
          }
      }
    }
 }

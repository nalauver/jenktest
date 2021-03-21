#!groovy
def remote = [:]
remote.allowAnyHosts = true
remote.appendName    = true
remote.host          = "10.10.10.10"
remote.identityFile  = "/home/jenkins/.ssh/id_rsa"
remote.knownHosts    = "/dev/null"
remote.name          = "neal-jenkin-test"
remote.pty           = true
remote.user          = "jenkins"

pipeline {
    agent { label "ec2-fleet" }

    environment {
       SOME_ENV_VAR = "blah"
    }

    stages {
        stage('GetPublicIP') {
            steps {
                 sh "curl http://169.254.169.254/latest/meta-data/public-ipv4 > commandResult"
                 result = readFile('commandResult').trim()
            }
        }
        stage('Build') {
            steps {
                git branch: 'master', url: 'https://github.com/nalauver/jenktest.git'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
   }
}

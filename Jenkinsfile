#!groovy
def remote = [:]
remote.allowAnyHosts = true
remote.appendName    = true
remote.host          = "10.10.10.10"
remote.identityFile  = "/home/jenktestuser/awscreds/nalauver.pem"
remote.knownHosts    = "/dev/null"
//remote.logLevel    = "INFO"
remote.name          = "test-aws-fleet"
remote.pty           = true
remote.user          = "ec2-user"

pipeline {
    agent { label "neal-local" }

    environment {
       SOME_ENV_VAR = "blah"
    }

    stages {
        stage('GetPublicIP') {
            agent { label "ec2-fleet" }
            steps {
                script {
                    remotehostip = sh ( script: 'curl http://169.254.169.254/latest/meta-data/public-ipv4', returnStdout: true )
                }
            }
        }
        stage('Build') {
            agent { label "ec2-fleet" }
            steps {
                git branch: 'master', url: 'https://github.com/nalauver/jenktest.git'
            }
        }
        stage('SshTest') {
            agent { label "neal-local" }
            steps {
                script {
                    echo "${remotehostip}"
                    remote.host = "${remotehostip}"
                }
                sshCommand remote: remote, command: "hostname -f"
            }
        }
        stage('Test') {
            agent { label "ec2-fleet" }
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            agent { label "ec2-fleet" }
            steps {
                echo 'Deploying....'
            }
        }
   }
}

#!groovy
def remote = [:]
remote.allowAnyHosts = true
remote.appendName    = true
remote.host          = "10.10.10.10"
remote.identityFile  = "/var/lib/jenkins/nalauv.er.pem"
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
                   env.PUBLICIP = sh 'curl http://169.254.169.254/latest/meta-data/public-ipv4'
                }
                echo "${env.PUBLICIP}"
            }
        }
        stage('Build') {
            agent { label "ec2-fleet" }
            steps {
                git branch: 'master', url: 'https://github.com/nalauver/jenktest.git'
            }
        }
        script {
            remote.host = env.PUBLICIP
        }
        stage('SshTest') {
            sshCommand remote: remote, command: "hostname -f"
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

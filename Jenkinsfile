pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_credentials')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_credentials')
    }

    stages {
        stage('Notification SLACK') {
            steps {
                slackSend channel: '#jenkins-slack-test', message: 'Posla zhara'
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                    credentialsId: 'git-hub_ssh-key',
                    url: 'git@github.com:Jennysiq/projectAPP.git'
                    ]]
                ])
            }
        }

        stage('Run shumba-bumba') {
            steps {
                sh 'ssh-add /home/ubuntu/.ssh/jenkins.pem'
            }
        }

        stage('run ec2.py') {
            steps {
                sh 'chmod +x ec2.py'
                sh './ec2.py --list'
            }
        }

        stage('Ansible') {
            steps {
                ansiblePlaybook become: true, becomeUser: 'root', credentialsId: 'jenkins', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ec2.py', playbook: 'playbook.yml'
            }
        }
    }
}


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
                sh 'echo shumba-bumba'
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
                ansiblePlaybook become: true, becomeUser: 'root', credentialsId: 'hw41', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ec2.py', playbook: 'deploy.yml'
            }
        }
    }
}

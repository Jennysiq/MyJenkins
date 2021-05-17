pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_credentials')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_credentials')
    }

    stages {
        stage('Notification SLACK') {
            steps {
                slackSend channel: '#jenkins-slack-test', message: 'Start Building Second Job'
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
        
        stage('Run Something') {
            steps {
                slackSend channel: '#jenkins-slack-test', message: 'Run something'
            }
        }
        
        stage('Build and push Container with myflask') {
            steps {
                sh "docker build -f Dockerfile . -t Jennysiq/myflask_project:${BUILD_ID}"
                sh "docker push Jennysiq/myflask_project:${BUILD_ID}"
            }
        }
        
        stage('Start .py dynamic inventory') {
            steps {
                slackSend channel: '#jenkins-slack-test', message: 'Dymanic Inventory Stage'
            }
        }
        
        stage('run ec2.py') {
            steps {
                sh 'chmod +x ec2.py'
                sh './ec2.py --list'
            }
        }

        stage('Delpoy apt through Ansible') {
            steps {
                slackSend channel: '#jenkins-slack-test', message: 'Delpoy apt through Ansible'
            }
        }
        
        stage('Ansible') {
            steps {
                ansiblePlaybook become: true, becomeUser: 'root', credentialsId: 'jenkins', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ec2.py', playbook: 'playbook.yml'
            }
        }
    }
}


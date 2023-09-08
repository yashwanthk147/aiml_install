pipeline {
    agent any

    environment {
        apppath = '/home/ubuntu'
        PYTHON_VERSION = '3.10'
        VENV_NAME = 'my-venv-name-3.10'
        SERVER_IP = '100.25.104.137'
    }

    stages {
        stage('Install Python and Setup Environment') {
            steps {
                sshagent(['terraform_id']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP}"
                    sh "scp -r /var/lib/jenkins/workspace/aiml-job/* ubuntu@${SERVER_IP}:${apppath}/"
                    sh "pwd"
                    sh "ls"
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        apppath = '/home/ubuntu'
        PYTHON_VERSION = '3.10'
        VENV_NAME = 'my-venv-name-3.10'
        SERVER_IP = '100.26.217.67'
    }

    stages {
        stage('Install Python and Setup Environment') {
            steps {
                sshagent(['terraform_id']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP}"

                    // Copy files from Jenkins workspace to remote server
                    sh "scp -o StrictHostKeyChecking=no -r /var/lib/jenkins/workspace/aiml-job/* ubuntu@${SERVER_IP}:${apppath}/"

                    // Set execute permission on the remote script
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'chmod +x ${apppath}/python_aiml.sh  && ./python_aiml.sh ${PYTHON_VERSION} ${VENV_NAME}'"
                    //sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && ./python_aiml.sh ${PYTHON_VERSION} ${VENV_NAME}'"

                    //activate source ~/.venvs/my-venv-name-3.9/bin/activate
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-3.10/bin/activate'"
                }
            }
        }
    }
}

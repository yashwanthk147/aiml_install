pipeline {
    agent any

    environment {
        apppath = '/home/ubuntu'
        //PYTHON_VERSION = '3.8'
        //VENV_NAME = 'my-venv-name-3.8'
        SERVER_IP = '100.26.217.67'
    }
    
    parameters{
        choice(name: 'PYTHON_VERSION', choices: ['3.8', '3.9', '3.10'], description: 'Pick Python Version') 
        choice(name: 'VENV_NAME', choices: ['my-venv-name-3.8', 'my-venv-name-3.9', 'my-venv-name-3.10'], description: 'Pick virtual environment name') 

    }
    stages {
        stage('Install Python and Setup Environment') {
            steps {
                sshagent(['terraform_id']) {

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'sudo rm -rf /home/ubuntu/*'"

                    // Copy files from Jenkins workspace to remote server
                    sh "scp -o StrictHostKeyChecking=no -r /var/lib/jenkins/workspace/aiml-job/* ubuntu@${SERVER_IP}:${apppath}/"

                    // Set execute permission on the remote script
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'chmod +x ${apppath}/python_aiml.sh  && ./python_aiml.sh ${params.PYTHON_VERSION} ${params.VENV_NAME}'"
                    //sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && ./python_aiml.sh ${PYTHON_VERSION} ${VENV_NAME}'"

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-${params.PYTHON_VERSION}/bin/activate'"
                   // sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-3.10/bin/activate'"
                }
            }
        }
    }
}

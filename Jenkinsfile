pipeline {
    agent any

    environment {
        apppath = '/home/ubuntu'

    }
    
    parameters{
        string(name: 'SERVER_IP', defaultValue: '100.26.217.67', description: 'EC2 Public IP')
        string(name: 'PRIVATE_IP', defaultValue: '100.26.217.67', description: 'EC2 Private IP')
        string(name: 'Domainname', defaultValue: 'nsfwattachementdetection-qa', description: 'Domain Name')
        choice(name: 'PYTHON_VERSION', choices: ['3.8', '3.9', '3.10'], description: 'Pick Python Version') 
        choice(name: 'VENV_NAME', choices: ['my-venv-name-3.8', 'my-venv-name-3.9', 'my-venv-name-3.10'], description: 'Pick virtual environment name') 

    }
    stages {
        stage('Install Python and Setup Environment') {
            steps {
                sshagent(['terraform_id']) {

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${params.SERVER_IP} 'sudo rm -rf ${apppath}/*'"

                    // Copy files from Jenkins workspace to remote server
                    sh "scp -o StrictHostKeyChecking=no -r /var/lib/jenkins/workspace/aiml-job/* ubuntu@${params.SERVER_IP}:${apppath}/"

                    // Set execute permission on the remote script
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${params.SERVER_IP} 'chmod +x ${apppath}/python_aiml.sh  && ./python_aiml.sh ${params.PYTHON_VERSION} ${params.VENV_NAME} ${params.Domainname} ${params.PRIVATE_IP}'"
                    //sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && ./python_aiml.sh ${PYTHON_VERSION} ${VENV_NAME}'"

                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${params.SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-${params.PYTHON_VERSION}/bin/activate'"
                   // sh "ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-3.10/bin/activate'"
                }
            }
        }
    }
}

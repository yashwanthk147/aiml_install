pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.10'
        VENV_NAME = 'my-venv-name-3.10'
        SERVER_IP = '35.175.148.74'
    }

    stages {
        stage('Install Python and Setup Environment') {
            steps {
                script {
                    sshagent(credentials: ['terraform_id']) {
                        sh """
                            pwd
                            ls
                            ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} ./python_aiml.sh ${PYTHON_VERSION} ${VENV_NAME}
                        """
                    }
                }
            }
        }
    }
}

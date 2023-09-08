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
                            ssh -o StrictHostKeyChecking=no ubuntu@${SERVER_IP} ./install_python.sh ${PYTHON_VERSION} ${VENV_NAME}
                        """
                    }
                }
            }
        }
    }
}

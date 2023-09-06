pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.10' // Set your desired Python version here
        VENV_NAME = 'my-venv-name-3.10' // Set your desired virtual environment name here
        SERVER_IP = '34.207.68.82'
    }

    stages {
        stage('Install Python and Setup Environment') {
            steps {
                script {
                    // Run the shell script with environment variables
                    sshCommand = "ssh -i pemkey ubuntu@${SERVER_IP} ./install_python.sh ${PYTHON_VERSION} ${VENV_NAME}"
                    sh sshCommand
                }
            }
        }
    }

    post {
        success {
            // post-build actions here
        }
    }
}

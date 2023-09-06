pipeline {
    agent any
    
    environment {
        PYTHON_VERSION = '3.10' // Set your desired Python version here
        VENV_NAME = 'my-venv-name-3.10' // Set your desired virtual environment name here
        server_ip = '34.207.68.82'
    }
    
    stages {
        stage() {
            steps {
                git checkout
            }
        }

        stage('Install Python and Setup Environment') {
            steps {
                script {
                    // Run the shell script with environment variables
                    sh '''
                       ssh -i pemkey ubuntu@ip
                        ./install_python.sh $PYTHON_VERSION $VENV_NAME
                    '''
                }
            }
        }
        
    }
    
    post {
        success {
            // You can add post-build actions here
        }
    }
}

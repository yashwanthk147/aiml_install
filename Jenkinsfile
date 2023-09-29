pipeline {
    agent any

    environment {
        apppath = '/home/ubuntu'
    }
    
    parameters{
        string(name: 'SERVER_IP', defaultValue: '34.229.137.44', description: 'EC2 Public IP')
        string(name: 'PRIVATE_IP', defaultValue: '172.31.58.217', description: 'EC2 Private IP')
        string(name: 'PRIVATE_KEY_S3_PATH', defaultValue: 'awspemkeys/web.pem', description: 'S3 path to private key')
        string(name: 'PRIVATE_KEY_PATH', defaultValue: '/var/lib/jenkins/workspace/aiml-python/web.pem', description: 'Local path to save private key')
        string(name: 'Domainname', defaultValue: 'grammercheckpt', description: 'Domain Name')
        choice(name: 'PYTHON_VERSION', choices: ['3.8', '3.9', '3.10'], description: 'Pick Python Version') 
        choice(name: 'VENV_NAME', choices: ['my-venv-name-3.8', 'my-venv-name-3.9', 'my-venv-name-3.10'], description: 'Pick virtual environment name') 

    }
    stages {
        stage('bit-bucket-checkout'){
            steps{
                
                git branch: 'main', credentialsId: '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', url: 'https://github.com/yashwanthk147/aiml_install.git'
            }

        }
        stage('Install Python and Setup Environment') {
            steps {
			   withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'awskey', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
					sh "aws s3 cp s3://${params.PRIVATE_KEY_S3_PATH} ${params.PRIVATE_KEY_PATH}"
					sh "chmod 400 ${params.PRIVATE_KEY_PATH}"
					sh "scp -o StrictHostKeyChecking=no -r -i ${params.PRIVATE_KEY_PATH} /var/lib/jenkins/workspace/aiml-install-script/* ubuntu@${params.SERVER_IP}:${apppath}/"
					sh "ssh -o StrictHostKeyChecking=no -i ${params.PRIVATE_KEY_PATH} ubuntu@${params.SERVER_IP} 'chmod +x ${apppath}/python_aiml.sh  && ./python_aiml.sh ${params.PYTHON_VERSION} ${params.VENV_NAME} ${params.Domainname} ${params.PRIVATE_IP}'"
					sh "ssh -o StrictHostKeyChecking=no -i ${params.PRIVATE_KEY_PATH} ubuntu@${params.SERVER_IP} 'cd ${apppath} && source ~/.venvs/my-venv-name-${params.PYTHON_VERSION}/bin/activate'"
                }
            }
        }
    }
}

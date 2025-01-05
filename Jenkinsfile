pipeline {
    agent {
        label 'jenkins-agent'
    }
    
    environment {
        AZURE_CREDS = credentials('azure-credentials')
    }
    
    stages {
        stage('Check Prerequisites') {
            steps {
                sh '''
                    which az || (echo "Azure CLI not found" && exit 1)
                '''
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/laxmiprasanna29393/jenkins-azure-vm.git'
            }
        }
        
        stage('Azure Login') {
            steps {
                sh '''
                    az login --service-principal \
                    --username "$AZURE_CREDS_USR" \
                    --password "$AZURE_CREDS_PSW" \
                    --tenant "dbd4e666-7a1b-4746-aa9d-a5e31edeac22"
                '''
            }
        }
        
        stage('Deploy VM') {
            steps {
                sh '''
                    az vm create \
                    --resource-group learn-jenkins \
                    --name jenkins-vm-${BUILD_NUMBER} \
                    --image Ubuntu2204 \
                    --admin-username azureuser \
                    --generate-ssh-keys
                '''
            }
        }
    }
    
    post {
        always {
            sh 'az logout'
        }
    }
}

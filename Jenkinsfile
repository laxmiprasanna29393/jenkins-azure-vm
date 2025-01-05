pipeline {
    agent any
    
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
                    -u $AZURE_CREDS_CLIENT_ID \
                    -p $AZURE_CREDS_CLIENT_SECRET \
                    --tenant $AZURE_CREDS_TENANT_ID
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

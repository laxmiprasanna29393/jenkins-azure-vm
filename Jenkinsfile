pipeline {
    agent any
    
    environment {
        AZURE_SUBSCRIPTION_ID = credentials('AZURE_SUBSCRIPTION_ID')
        AZURE_TENANT_ID = credentials('AZURE_TENANT_ID')
        AZURE_CLIENT_ID = credentials('AZURE_CLIENT_ID')
        AZURE_CLIENT_SECRET = credentials('AZURE_CLIENT_SECRET')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'YOUR_GITHUB_REPO_URL'
            }
        }
        
        stage('Azure Login') {
            steps {
                sh '''
                    az login --service-principal \
                    -u $AZURE_CLIENT_ID \
                    -p $AZURE_CLIENT_SECRET \
                    --tenant $AZURE_TENANT_ID
                '''
            }
        }
        
        stage('Deploy VM') {
            steps {
                sh '''
                    az vm create \
                    --resource-group jenkins-rg \
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

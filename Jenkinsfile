pipeline {
    agent any

    parameters {
        string(name: 'RESOURCE_GROUP', defaultValue: 'tf-rg', description: 'Azure Resource Group')
        string(name: 'LOCATION', defaultValue: 'East US', description: 'Azure Location')
        string(name: 'VM_NAME', defaultValue: 'jenkins-vm', description: 'VM Name')
        string(name: 'VM_SIZE', defaultValue: 'Standard_B1s', description: 'Azure VM Size')
    }

    environment {
        ARM_CLIENT_ID       = credentials('AZURE_CLIENT_ID')
        ARM_CLIENT_SECRET   = credentials('AZURE_CLIENT_SECRET')
        ARM_SUBSCRIPTION_ID = credentials('AZURE_SUBSCRIPTION_ID')
        ARM_TENANT_ID       = credentials('AZURE_TENANT_ID')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-repo>/terraform-azure-vm.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh """
                terraform init
                """
            }
        }

        stage('Terraform Plan') {
            steps {
                sh """
                terraform plan \
                  -var resource_group_name=${params.RESOURCE_GROUP} \
                  -var location="${params.LOCATION}" \
                  -var vm_name=${params.VM_NAME} \
                  -var vm_size=${params.VM_SIZE} \
                  -var subscription_id=$ARM_SUBSCRIPTION_ID \
                  -var client_id=$ARM_CLIENT_ID \
                  -var client_secret=$ARM_CLIENT_SECRET \
                  -var tenant_id=$ARM_TENANT_ID
                """
            }
        }

        stage('Terraform Apply') {
            steps {
                sh """
                terraform apply -auto-approve \
                  -var resource_group_name=${params.RESOURCE_GROUP} \
                  -var location="${params.LOCATION}" \
                  -var vm_name=${params.VM_NAME} \
                  -var vm_size=${params.VM_SIZE} \
                  -var subscription_id=$ARM_SUBSCRIPTION_ID \
                  -var client_id=$ARM_CLIENT_ID \
                  -var client_secret=$ARM_CLIENT_SECRET \
                  -var tenant_id=$ARM_TENANT_ID
                """
            }
        }
    }
}

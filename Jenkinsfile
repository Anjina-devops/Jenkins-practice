pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your repository
                git 'https://github.com/Anjina-devops/roboshop-project-configuration.git'

                sh '''
                    ls -ltr
                    pwd
                    cd /terraform/ec2
                    ls -l
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                echo 'Initializing Terraform...'
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                // Create a Terraform plan
                echo 'Creating Terraform plan...'
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Approval') {
            steps {
                // Wait for manual approval before applying changes
                script {
                    def userInput = input(
                        id: 'userInput', // Unique id for the input step
                        message: 'Do you want to apply the Terraform plan?',
                        parameters: [
                            [$class: 'BooleanParameterDefinition', name: 'Approve', defaultValue: true]
                        ]
                    )
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                // Apply the Terraform plan
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        success {
            echo 'Terraform apply was successful!'
        }
        failure {
            echo 'Terraform apply failed.'
        }
    }
}
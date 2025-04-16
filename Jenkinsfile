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
                    cd ec2
                    ls -l
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                echo 'Initializing Terraform...'
                sh '''
                cd ec2
                terraform init
                terraform plan
        '''
            }
        }

        stage('Terraform Plan') {
            steps {
                // Create a Terraform plan
                echo 'Creating Terraform plan...'
                sh '''
                cd ec2
                
                terraform plan
        '''
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
               sh '''
                cd ec2
                echo 'Applying Terraform plan...with dummy code'
                terraform apply -auto-approve
            '''
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
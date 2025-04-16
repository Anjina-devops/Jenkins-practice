pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
               

                sh '''
                    ls -ltr
                    pwd
                   
                    ls -l
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                echo 'Initializing Terraform...'
                
            }
        }

        stage('Terraform Plan') {
            steps {
                // Create a Terraform plan
                echo 'Creating Terraform plan...'
                
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
                echo 'Applying Terraform plan...with dummy code'
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
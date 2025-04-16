pipeline {
    agent { label 'AGENT-1' }

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
                // Initialize Terraform yhrough jenkins pipeline
                echo 'Initializing Terraform...'
                sh '''
                cd ec2
                terraform init
                '''
            }
        }

        stage('Terraform Plan') {
            steps {
                dir('ec2') {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-auth']]) {
                        sh 'terraform plan'
                    }
                }
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
                dir('ec2') {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-auth']]) {
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }

        stage('Approval') {
            steps {
                // Wait for manual approval before applying changes
                script {
                    def userInput = input(
                        id: 'userInput', // Unique id for the input step
                        message: 'Do you want to Destroy the Terraform plan?',
                        parameters: [
                            [$class: 'BooleanParameterDefinition', name: 'Approve', defaultValue: true]
                        ]
                    )
                }
            }
        }

        stage('Terraform Destroy') {
            steps {
                dir('ec2') {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-auth']]) {
                        sh 'terraform destroy -auto-approve'
                    }
                }
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
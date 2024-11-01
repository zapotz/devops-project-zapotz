pipeline {
    agent any

    parameters {
        booleanParam(name: 'PLAN_TERRAFORM', defaultValue: false, description: 'Check to plan Terraform changes')
        booleanParam(name: 'APPLY_TERRAFORM', defaultValue: false, description: 'Check to apply Terraform changes')
        booleanParam(name: 'DESTROY_TERRAFORM', defaultValue: false, description: 'Check to destroy Terraform resources')
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Limpiar workspace antes de clonar (opcional)
                deleteDir()

                // Clonar el repositorio de Git
                git branch: 'main', url: 'https://github.com/zapotz/devops-project-zapotz.git'

                sh "ls -lart"
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-crendential-zapotz']]) {
                    dir('infra') {
                        sh 'echo "=================Terraform Init=================="'
                        sh 'terraform init'
                    }
                }
            }
        }

        stage('Terraform Plan') {
            when {
                expression {
                    return params.PLAN_TERRAFORM
                }
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-crendential-zapotz']]) {
                    dir('infra') {
                        sh 'echo "=================Terraform Plan=================="'
                        sh 'terraform plan'
                    }
                }
            }
        }

        stage('Terraform Apply') {
            when {
                expression {
                    return params.APPLY_TERRAFORM
                }
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-crendential-zapotz']]) {
                    dir('infra') {
                        sh 'echo "=================Terraform Apply=================="'
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }

        stage('Terraform Destroy') {
            when {
                expression {
                    return params.DESTROY_TERRAFORM
                }
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-crendential-zapotz']]) {
                    dir('infra') {
                        sh 'echo "=================Terraform Destroy=================="'
                        sh 'terraform destroy -auto-approve'
                    }
                }
            }
        }
    }
}

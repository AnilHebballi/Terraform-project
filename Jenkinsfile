pipeline {
    agent any
    parameters {
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Terraform action to run')
    }
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out Code From GitHub"
                git branch: 'main', url: 'https://github.com/AnilHebballi/Terraform-project.git'
            }
        }
        stage('Terraform Init') {
            steps {
                echo "Initializing Terraform"
                sh 'terraform init'
            }
        }
        stage('Terraform Validate') {
            steps {
                echo "Validating the Terraform Configuration"
                sh 'terraform validate'
            }
        }
        stage('Terraform Plan') {
            when {
                expression { params.action == 'apply' }
            }
            steps {
                echo "Running Terraform Plan"
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Terraform Apply') {
            when {
                expression { params.action == 'apply' }
            }
            steps {
                echo "Applying Terraform Configuration"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
        stage('Terraform Destroy') {
            when {
                expression { params.action == 'destroy' }
            }
            steps {
                echo "Destroying Terraform Managed Infrastructure"
                sh 'terraform destroy -auto-approve'
            }
        }
    }
    post {
        success {
            echo "Terraform action completed successfully"
        }
        failure {
            echo "Terraform action failed"
        }
    }
}

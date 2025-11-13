pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git 'https://github.com/user/myapp.git' }
        }
        stage('Build') {
            steps { sh 'mvn clean package' }
        }
        stage('Docker Build & Push') {
            steps {
                sh "docker build -t myapp ."
                sh "docker tag myapp:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest"
                sh "docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest"
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh "kubectl set image deployment/myapp-deployment myapp=<account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest"
            }
        }
    }
}

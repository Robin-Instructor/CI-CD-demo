pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '046402772087'
        ECR_REPO_NAME = 'test'
    }
    
    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/Robin-Instructor/CI-CD-demo.git'
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:latest")
                }
            }
        }
        stage('Push Docker image to ECR') {
            steps {
                script {
                    docker.withRegistry('', 'ecr:us-east-1') {
                        docker.image("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:latest").push()
                    }
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                script {
                    kubernetesDeploy(
                        kubeconfigId: 'your-kubeconfig-credentials',
                        configs: 'deployment.yaml',
                        enableConfigSubstitution: true
                    )
                    kubernetesDeploy(
                        kubeconfigId: 'your-kubeconfig-credentials',
                        configs: 'service.yaml',
                        enableConfigSubstitution: true
                    )
                }
            }
        }
    }
}

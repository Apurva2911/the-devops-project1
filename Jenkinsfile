pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "apurva2911/flask-app:latest"
        AWS_REGION = "us-east-1"
        EKS_CLUSTER = "mycluster"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Apurva2911/the-devops-project1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh './scripts/deploy.sh'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh "kubectl get svc"
            }
        }
    }

    post {
        success {
            echo " CI/CD Pipeline finished successfully!"
        }
        failure {
            echo " CI/CD Pipeline failed. Check logs!"
        }
    }
}

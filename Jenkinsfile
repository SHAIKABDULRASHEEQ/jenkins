pipeline {
    agent any

    environment {
        IMAGE_NAME = "shaikabdulrasheeq/cicd-demo"
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SHAIKABDULRASHEEQ/jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                kubectl rollout status deployment/backend
                """
            }
        }
    }
}

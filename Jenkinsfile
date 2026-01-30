pipeline {
    agent any

    environment {
        IMAGE_NAME = "cicd-demo"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }

}
        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$DOCKER_TAG .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                sed -i "s|IMAGE_NAME|$IMAGE_NAME:$DOCKER_TAG|g" deployment.yaml
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}


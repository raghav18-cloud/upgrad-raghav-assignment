pipeline {
    agent any
    environment {
        ECR_REGISTRY = "614217881944.dkr.ecr.us-east-1.amazonaws.com"
        IMAGE_NAME = "node-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Git Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raghav18-cloud/upgrad-raghav-assignment.git']]])
            }
        }
        stage('Build and Publish Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'ecr-creds', variable: 'DOCKER_CREDENTIALS')]) {
                    sh '''
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                        $(aws ecr get-login --no-include-email)
                        docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                        docker push ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }
}



pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        AWS_REGION = 'us-east-1'
        REGISTRY = '971615377317.dkr.ecr.us-east-1.amazonaws.com'
        IMAGE_NAME = 'petclinic-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/praveenudemycourses/spring-petclinic'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag $DOCKER_IMAGE $REGISTRY/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $REGISTRY
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker push $REGISTRY/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}

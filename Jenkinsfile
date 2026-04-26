pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t petclinic-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8082:8080 petclinic-app'
            }
        }
    }
}

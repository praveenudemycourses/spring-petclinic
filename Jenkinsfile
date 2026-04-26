pipeline {
  agent any

  tools {
    maven 'maven'
  }

  stages {

    stage('Build & Test') {
      steps {
        sh 'mvn clean verify'
      }
    }
    stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('sonarqube') {
            sh 'mvn sonar:sonar'
        }
    }
}


    stage('Quality Gate') {
      steps {
        timeout(time: 4, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }

  }

  post {
    success {
      archiveArtifacts artifacts: 'target/*.jar'
    }
  }
}

pipeline {
  agent any
  environment {
    SONAR_TOKEN = credentials('sonar-qube-token')
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build') { steps { sh './mvnw -B -DskipTests clean package' } }
    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh "./mvnw sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${SONAR_TOKEN}"
        }
      }
    }
    stage('Quality Gate') {
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
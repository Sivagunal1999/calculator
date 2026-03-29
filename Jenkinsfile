pipeline {
  agent any
  stages {

    stage('Compile') {
      steps{
        sh 'mvn clean compile'
      }
    }

    stage('UnitTest') {
      steps{
        sh 'mvn clean test'
      }
    }

     stage('Package') {
      steps{
        sh 'mvn clean package'
      }
    }

     stage('Package2') {
      steps{
        sh 'mvn clean package2'
      }
    }

    stage('Deliver') {
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8081/')], contextPath: '/calculator', war: 'target/calculator.war'
      }
    }
  }
}

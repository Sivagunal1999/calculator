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

    stage('Deliver') {
      steps{
        deploy adapters: [tomcat10(credentialsId: 'tomcat', path: '', url: 'http://172.31.91.20:9090/')], contextPath: null, war: 'target/calculator.war'
      }
    }
  }
}

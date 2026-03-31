pipeline {
    agent any
    stages {
        stage('Checkout Develop') {
            steps {
                // This fulfills the "pull to a folder" requirement
                dir('develop-branch-code') {
                    git branch: 'Develop', 
                        url: 'https://github.com/Sivagunal1999/calculator.git'
                }
            }
        }

        stage('Compile') {
            steps {
                dir('develop-branch-code') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage('UnitTest') {
            steps {
                dir('develop-branch-code') {
                    sh 'mvn test' // Removed clean to save time
                }
            }
        }

        stage('Package') {
            steps {
                dir('develop-branch-code') {
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Deliver') {
            steps {
                dir('develop-branch-code') {
                    deploy adapters: [
                        tomcat9(credentialsId: 'tomcat', url: 'http://localhost:8081/')
                    ], 
                    contextPath: '/calculator', 
                    war: 'target/calculator.war'
                }
            }
        }
    }
}

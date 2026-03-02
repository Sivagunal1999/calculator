📘 CI/CD Pipeline Deployment to Tomcat – Issue & Fix Documentation
This document explains the step-by-step setup and troubleshooting performed to deploy a Maven .war application from Jenkins to Apache Tomcat 10 using a Jenkins Pipeline.

🎯 Objective

Automate the following flow:
Source Code → Maven Build → WAR Package → Deploy to Tomcat 10

🧱 Initial Setup (From Scratch)
1️⃣ Installed Java 21
Tomcat 10 requires Java 11+.

Verified installation:
java -version

Confirmed:
OpenJDK 21

2️⃣ Installed Apache Tomcat 10
Extracted Tomcat under:
/opt/tomcat

Changed default port from 8080 → 8081
(to avoid conflict with Jenkins)

Edited:
/opt/tomcat/conf/server.xml

Changed:
<Connector port="8080"

To:
<Connector port="8081"

3️⃣ Started Tomcat
/opt/tomcat/bin/startup.sh

Verified in browser:
http://<server-ip>:8081
Tomcat home page loaded successfully.

4️⃣ Configured Tomcat Manager Access

Edited:
/opt/tomcat/conf/tomcat-users.xml

Added:
<role rolename="manager-script"/>
<role rolename="manager-gui"/>
<user username="admin" password="admin123" roles="manager-script,manager-gui"/>

Restarted Tomcat.
This allows Jenkins to deploy WAR files using manager API.

5️⃣ Installed Required Jenkins Plugin

Installed:
Deploy to Container Plugin

Path:
Manage Jenkins → Manage Plugins

6️⃣ Added Credentials in Jenkins

Path:
Manage Jenkins → Credentials → Global → Add Credentials

Added:
Username: tomcat
Password: tomcat
ID: tomcat

❌ Error Faced During Pipeline Execution

Build failed with error:

No such DSL method 'tomcat10'
🔎 Root Cause
The Jenkins Deploy Plugin does NOT support:
tomcat10()

Supported adapters available were:
tomcat4
tomcat5
tomcat6
tomcat7
tomcat8
tomcat9

Even though Tomcat 10 was installed, the plugin still expects tomcat9().

✅ Fix Implemented
Replaced:
deploy adapters: [tomcat10(
With:
deploy adapters: [tomcat9(

This works because the underlying deployment API is compatible.

🛠️ Final Working Jenkinsfile
pipeline {
  agent any

  stages {

    stage('Compile') {
      steps {
        sh 'mvn clean compile'
      }
    }

    stage('UnitTest') {
      steps {
        sh 'mvn clean test'
      }
    }

    stage('Package') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Deploy') {
      steps {
        deploy adapters: [tomcat9(
          credentialsId: 'tomcat',
          path: '',
          url: 'http://localhost:8081/'
        )],
        contextPath: '/calculator',
        war: 'target/calculator.war'
      }
    }
  }
}
🎉 Final Result

Build successful

WAR generated successfully

Deployment completed without errors

Application accessible at:

http://<server-ip>:8081/calculator

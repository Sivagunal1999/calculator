✅ 1️⃣ Install Java (if not installed)
sudo apt update
sudo apt install openjdk-21-jdk -y
java -version

✅ 2️⃣ Download Tomcat
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.86/bin/apache-tomcat-9.0.86.tar.gz

✅ 3️⃣ Extract Tomcat
tar -xvzf apache-tomcat-9.0.86.tar.gz

✅ 4️⃣ Move to /opt
sudo mv apache-tomcat-9.0.86 /opt/tomcat

✅ 5️⃣ Give Execute Permission
sudo chmod +x /opt/tomcat/bin/*.sh

✅ 6️⃣ Start Tomcat (Default 8080)
sudo /opt/tomcat/bin/startup.sh

Check running:

ps -ef | grep tomcat
✅ 7️⃣ Change Port (8080 → 8081)

Because Jenkins was using 8080.

Edit:
sudo nano /opt/tomcat/conf/server.xml

Find:
<Connector port="8080"

Change to:
<Connector port="8081"

Save and restart:
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh

Access:
http://<EC2-IP>:8081
✅ 8️⃣ Add Tomcat Username & Password

Edit:
sudo nano /opt/tomcat/conf/tomcat-users.xml

Add before </tomcat-users>:
<role rolename="manager-script"/>
<role rolename="manager-gui"/>
<user username="tomcat" password="tomcat"
      roles="manager-script,manager-gui"/>

Restart Tomcat again:
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh

✅ 9️⃣ Fix 403 Access Denied (Allow Remote Access)

Edit:
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml

Comment this block:
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1" />
-->

Restart Tomcat:
sudo /opt/tomcat/bin/shutdown.sh
sudo /opt/tomcat/bin/startup.sh

Now Manager works:
http://<EC2-IP>:8081/manager/html

✅ 🔟 Deploy WAR Manually

Build project:
cd ~/calculator
mvn clean package

Copy WAR:
sudo cp target/calculator.war /opt/tomcat/webapps/

Access:
http://<EC2-IP>:8081/calculator

Testing Webhook at 2026-03-31
Trigger Test
After updating JenkinsFile
After updating the Jenkinsfile again
Trigger Test #2

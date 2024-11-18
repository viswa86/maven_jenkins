# tomcat-project


Apache tomcat: It is a webserver, it is used to execute java servlets and render javaserver pages.
Installation in ubuntu:

# Firstly update your system packages

sudo apt update

# The java version supported is 8 or above. Here we are getting Java 11
.
sudo apt install default-jdk

# To check and confirm, that Java has been installed successfully.

java -version

Output:
openjdk version "11.0.19" 2023-04-18
OpenJDK Runtime Environment (build 11.0.19+7-post-Ubuntu-0ubuntu122.04.1)
OpenJDK 64-Bit Server VM (build 11.0.19+7-post-Ubuntu-0ubuntu122.04.1, mixed mode, sharing)

# Installing Apache Tomcat

Sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.97/bin/apache-tomcat-9.0.97.tar.gz.

# Create a directory under /opt.

Sudo mkdir -p /opt/tomcat

# Extract the downloaded Tomcat tar.

Sudo tar xzvf apache-tomcat-9.0.97.tar.gz

# Create a dedicated user

Sudo groupadd tomcat
Sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

# Give Tomcat user permission

Sudo chown -R tomcat: /opt/tomcat
Sudo sh -c ‘chmod +x /opt/tomcat/bin/*.sh’

# Create a Systemd service file

sudo nano /etc/systemd/system/tomcat.service
Paste the following block of code in it
[Unit]
Description=Tomcat webs servlet container
After=network.target
[Service]
Type=forking
User=tomcat
Group=tomcat
RestartSec=10
Restart=always
Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh
[Install]
WantedBy=multi-user.target
To save the press Ctrl+X, type –Y, and hit the Enter Key.

# Start and Enable the Tomcat service
sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl enable tomcat.
#Find the public addres using
curl ifconfig.me

# Change the username and password
sudo nano /opt/tomcat/conf/tomcat-users.xml
<role rolename="admin-gui,manager-gui,manager-script,manager-jmx,manager-status,admin-gui"/>
  <user username="admin" password="password" roles="admin-gui,manager-gui,manager-script"/>
  
 # Enable Tomcat and Host Manager Remote access
For Tomcat Manager’s remote access:
Edit the Context file  
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
Comment out “<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -- > “
For Host manager remote access:
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
Just like above,

# If you need change the port to 9090
Sudo nano /opt/tomcat/conf/server.xml
<Connector port="9090" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
Using curl command lets download
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
Now lets update system packages:
sudo apt update
Now Install Jenkins:
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
http://server-ip-addres:8080
a138ef633fd6c64cf764bd7b3a6790ab3783193d


## NEXUS INSTALLATION.


**1. Create a Virtual Machine using AZURE or AWS.**

**2. First, update your package lists and upgrade existing packages. We also need to install wget, which will be used to download the Nexus tar file.**
```
sudo apt update -y
sudo apt upgrade -y
sudo apt install wget -y
```
**3.Install JAVA**
```
java -version
sudo apt install openjdk-11-jre-headless
```

**4. Login as su**
```
sudo su
```

```
cd /opt
```
**5. Download and extract nexus**
```
wget https://download.sonatype.com/nexus/3/nexus-3.74.0-05-unix.tar.gz
tar -xvzf nexus-3.74.0-05-unix.tar.gz
```

**6. Chnage the name of the file.**
```
mv nexus-3.74.0-05 nexus
```
**7. Create a new user by the name of 'nexus'**

```
adduser nexus
```
**8. To disable the password of the nexus**

```
visudo
```
  Add this following line under the root.
```
nexus ALL=(ALL) NOPASSWD: ALL
```
**9. Setting up permissions for the created directories.**

```
chown -R nexus:nexus nexus
chown -R nexus:nexus sonatype-work/
```

**10. Edit the nexus.rc file to specify that Nexus should run as the nexus user:**

```
nano /opt/nexus/bin/nexus.rc
run_as_user="nexus"
```
**11. Create a Systemd Service File**

```
nano /etc/systemd/system/nexus.service

[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abortTimeoutSec=600

[Install]
WantedBy=multi-user.target
```
**12. Start and Enable Nexus Service**

```
sudo systemctl daemon-reload
sudo systemctl start nexus
sudo systemctl enable nexus
```

Check the status of the nexus service.
```
systemctl status nexus.service
```
**13. Access Nexus Web Interface**

```
http://<server_ip>:8081
```
**14. To fetch the login password**

```
cat sonatype-work/nexus3/admin.password
```

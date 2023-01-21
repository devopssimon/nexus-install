1. SSH into the Nexus Server
2. yum update -y
3. yum install nano
4. yum install -y java-1.8.0-openjdk.x86_64
4. mkdir /app
5. cd /app
6. wget -O nexus.tar.gz https://download.sonatype.com/nexus/3/latest-unix.tar.gz
6. yum install tar -y
7. tar -xvf nexus.tar.gz
8. mv nexus-3* nexus
9. adduser nexus
10. chown -R nexus:nexus /app/nexus 
11.	chown -R nexus:nexus /app/sonatype-work
12.	nano /app/nexus/bin/nexus.rc
13.	run_as_user="nexus"
14.	df -h and look for the name of the mounted volume /mnt/HC_Volume_*
15.	mkdir /mnt/HC_Volume_26908811/nexus
16.	chown nexus:nexus /mnt/HC_Volume_26908811/nexus/
17.	nano /app/nexus/bin/nexus.vmoptions
18.	Change -Dkaraf.data=/nexus/nexus-data to mounted volume path
19.	nano /etc/systemd/system/nexus.service add the content below

```
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
Group=nexus
ExecStart=/app/nexus/bin/nexus start
ExecStop=/app/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
20.	systemctl start nexus
21.	systemctl enable nexus
22.	Get password with cat /app/sonatype-work/nexus3/admin.password

Open the Browser
Navigate to http://ip-address:8081
Login with Admin and the password you copied
Change password
Select Enable anonymous access

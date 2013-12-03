Avoid installing as root.

`$ apt-get update && apt-get upgrade`

`$ apt-get install ssh denyhosts openjdk-7-jre`

`$ adduser wwwuser`

Edit the configuration to suit your needs
 
`$ nano /etc/hosts.allow` add `sshd: 1.3.4.5`

`$ nano /etc/ssh/sshd_config` add `PermitRootLogin no`

`$ /etc/init.d/denyhosts restart`

`$ reload ssh`
 
SSH with the appropriate user

`$ whoami`

`wwwuser`

Download GitBucket, which ever version you want

`$ cd ~`

`$ mkdir gitbucket`

`$ mkdir gitbucket-binary`

`$ cd gitbucket-binary`

`$ wget -O gitbucket-1.6.war https://github.com/takezoe/gitbucket/releases/download/1.6/gitbucket.war`
`$ wget -O gitbucket-1.7.war https://github.com/takezoe/gitbucket/releases/download/1.7/gitbucket.war`
`$ wget -O gitbucket-1.8.war https://github.com/takezoe/gitbucket/releases/download/1.8/gitbucket.war`

To run GitBucket

`$ java -jar gitbucket-1.8.war`

To install GitBucket

`$ cd /usr/local/bin/`

Create a start script 

`$ sudo nano gitbucket-start.sh`

```sh
 #!/bin/bash
 
 #sudo -u wwwuser -H java -jar /home/wwwuser/gitbucket-binary/gitbucket-1.6.war &
 #sudo -u wwwuser -H java -jar /home/wwwuser/gitbucket-binary/gitbucket-1.7.war &
 sudo -u wwwuser -H java -jar /home/wwwuser/gitbucket-binary/gitbucket-1.8.war &
```

Create a stop script

`$ sudo nano gitbucket-stop.sh`

```sh
 #!/bin/bash
 
 pid=`ps aux | grep gitbucket | awk '{print $2}'`
 kill -9 $pid
```

Create the start-up script in `init.d`

`$ nano /etc/init.d/gitbucket`

```sh
 #!/bin/bash
 
 case $1 in
    start)
        /bin/bash /usr/local/bin/gitbucket-start.sh
    ;;
    stop)
        /bin/bash /usr/local/bin/gitbucket-stop.sh
    ;;
    restart)
        /bin/bash /usr/local/bin/gitbucket-stop.sh
        /bin/bash /usr/local/bin/gitbucket-start.sh
    ;;
 esac
 exit 0
```

Mark all scripts as executable
 
`$ sudo chmod 755 /usr/local/bin/gitbucket-start.sh`

`$ sudo chmod 755 /usr/local/bin/gitbucket-stop.sh`

`$ sudo chmod 755 /etc/init.d/gitbucket`

Set the script to startup using SysV

`$ sudo update-rc.d gitbucket defaults`

Test

`$ sudo reboot`





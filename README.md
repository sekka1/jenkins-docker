# jenkins-docker
[![GitHub forks](https://img.shields.io/github/forks/sekka1/jenkins-docker.svg)](https://github.com/sekka1/jenkins-docker/network)
[![GitHub stars](https://img.shields.io/github/stars/sekka1/jenkins-docker.svg)](https://github.com/sekka1/jenkins-docker/stargazers)
[![Twitter](https://img.shields.io/twitter/url/https/github.com/sekka1/jenkins-docker.svg?style=social)](https://twitter.com/intent/tweet?text=Jenkins%202.0%20in%20Production%20with%20Docker:&url=https://github.com/sekka1/jenkins-docker)


Running Jenkins 2.0 in Production with Docker.  

Nothing to install, the only prerequisite is that you have Docker installed on the server.  

Files persists onto the localhost server at `/opt/jenkins_home`.  This makes it resilient to reboots and it is easy to just backup this folder.

# Running the Jenkins service

## Ubuntu

#### 1) Place the file `./upstart/jenkins.conf` on your server `/etc/init/jenkins.conf`

#### 2) Start it up
```
service jenkins start
```
#### 3) Get the password from the log files
```
docker logs -ft jenkins
```
You will see this log section.  Copy this password:

```
2016-05-05T17:25:49.011787978Z *************************************************************
2016-05-05T17:25:49.011808021Z *************************************************************
2016-05-05T17:25:49.011814273Z *************************************************************
2016-05-05T17:25:49.011819429Z
2016-05-05T17:25:49.011824960Z Jenkins initial setup is required. An admin user has been created and a password generated.
2016-05-05T17:25:49.011834803Z Please use the following password to proceed to installation:
2016-05-05T17:25:49.011839910Z
2016-05-05T17:25:49.011845543Z e30bed7d3c104005aeadd45de3a60d37
2016-05-05T17:25:49.011850557Z
2016-05-05T17:25:49.011855603Z This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
2016-05-05T17:25:49.011860984Z
2016-05-05T17:25:49.011865623Z *************************************************************
2016-05-05T17:25:49.011871858Z *************************************************************
2016-05-05T17:25:49.011877088Z *************************************************************
```

#### 4) Go to this URL in your web browser  
Go to `http://localhost` (or whatever the IP of the server is)

You will need the password to go through the setup.

# Backup

## Manual
Just backup the folder `/opt/jenkins_home`.  If you need to restore, just restore all of the contents to this location and start up Jenkins again.

## To S3
You can backup the files to AWS S3 by running this Docker command:

```
docker run --env aws_key=<YOUR AWS KEY> \
--env aws_secret=<YOUR AWS SECRET> \
--env cmd=sync-local-to-s3 \
--env DEST_S3=s3://my.backup.bucket/jenkins_home/  \
-v /opt/jenkins_home:/opt/src/$(/bin/date +\%Y\%m\%d) \ garland/docker-s3cmd
```

## cron to backup to S3
You can also setup a cron job that uses the S3 backup on a schedule.

1) Edit your crontab

2) Add this entry in
```
0 22 * * *      /usr/bin/docker run --env aws_key=<YOUR AWS KEY> --env aws_secret=<YOUR AWS SECRET> --env cmd=sync-local-to-s3 --env DEST_S3=s3://my.backup.bucket/jenkins_home/  -v /opt/jenkins_home:/opt/src/$(/bin/date +\%Y\%m\%d) garland/docker-s3cmd
```

This will perform a backup nightly.

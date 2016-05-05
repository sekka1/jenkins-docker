# jenkins-docker
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

#### 4) Go to this URL in your web browser `http://localhost`.  
You will need the password to go through the setup.

# Backup

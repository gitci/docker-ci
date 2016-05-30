# JENKINS

Official image : [jenkins](https://hub.docker.com/_/jenkins/)


## Credentials
- Go to /configureSecurity/ and check "activate security"
- Choose your Realm (Jenkins usr database)
- Choose matrice based security
- Give anonymous the Global read permission
- Add an "admin" user with all permissions
- Save
- Click on "signing up" and create your admin account

## Configure Gitlab
> Make sure you created the Jenkins user in Gitlab first and you copy the "Private token" under the "Account" section

Go to /configure. In Gitlab section :
- Name default connection
- Set "Gitlab host URL" to http://gitlab.your.host/
- Add credential for jenkins :
    - Set to "Secret text"
    - Secret: Private Key from gitlab
    - Description: Gitlab API KEY
Repeat for "Gitlab Notifier Configuration" section

## Configure Sonar

Under "SonarQube servers" click on "Add a SonarQube installation"
- Name: "Default"
- URL: http://sonar.your.host/
- Server authentication token: Go to SonarQube to generate one ([doc](http://docs.sonarqube.org/display/SONAR/User+Token))

Under "SonarQube Scanner" click on "Add a SonarQube Scanner"
- Name: default
- Install automatically from Maven Central, version : 2.4


## Configure a job

After creating your first project on Gitlab you can create it's job on jenkins. To do this :

- Click on "create a new job"
- Set "Git" as the VCS
- Add credential for the git connection
    - Select "SSH Username with private key"
    - Username is the jenkins user on gitlab
    - Private Key is "From the jenkins master ~/.ssh"
- Check "Build when a change is pushed to GitLab. GitLab CI Service URL: your-project-url"

> You can now go to your gitlab project, click on "settings" and select "webhooks". Add "your-project-url" and select the notifications policy (push / merge request / etc...)

Now you can configure the job build steps

### SonarQube Scanner

For the analysis, select "SonarQube Scanner" build step and add properties for your project. Example :

```
sonar.gitlab.url=http://gitlab.your.host/
sonar.gitlab.project_id=test/silex
sonar.gitlab.commit_sha=0c858d32b2d5db0e3ad43c85d6993b0fa70b3422
sonar.gitlab.max_global_issues=100
sonar.gitlab.user_token=mFyymsRJJbEYsp5vLTHC

sonar.host.url=http://sonar.your.host/
sonar.login=e391fa903214f860f82b9126e43e740b843bfcd9
sonar.projectKey=test-silex
sonar.projectName=Silex
sonar.projectVersion=1.0

sonar.sources=src
sonar.language=php
sonar.sourceEncoding=UTF-8
```

> Make sure you created a "test-silex" project in SonarQube


### Build a docker project

Create a "shell script" task which contains the following commands :

```
# Build the application and run it
docker build -f app/env/app/Dockerfile --tag silex.app .
docker build -f app/env/php/Dockerfile --tag silex.php .
docker run -dit --name ci.silex.app silex.app
docker run -dit --volumes-from $(docker ps -f name=silex.app -q)  --name ci.silex.php silex.php

# Install project dependencies and launch unit tests
docker exec $(docker ps -f name=ci.silex.php -q) sh -c "cd /var/www/silex/ && composer update"
docker exec $(docker ps -f name=ci.silex.php -q) sh -c "cd /var/www/silex/ && composer run-script test"

# Clear docker containers and images
docker kill $(docker ps -f name=ci.silex.php -q)
docker kill $(docker ps -f name=ci.silex.app -q)
docker rm ci.silex.app
docker rm ci.silex.php
```

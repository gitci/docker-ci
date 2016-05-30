# GITLAB

More information on the gitlab docker image : [sameersbn/docker-gitlab](https://github.com/sameersbn/docker-gitlab)

## Configuring Jenkins user
- In the top right corner click on "Admin area"
- Go to "Users" section
- Click on "New user"
- Create a Jenkins user
- Click on impersonate
- Go to "SSH Keys"
- Add the jenkins public key in ./jenkins/config/ssh-keys
- Go to "Account" and copy the "Private Token" for later
- In top right corner click on "Stop Impersonation"

## Configuring Sonar user
- In the top right corner click on "Admin area"
- Go to "Users" section
- Click on "New user"
- Create a Sonar user
- Click on impersonate
- Go to "Account" and copy the "Private Token" for later
- In top right corner click on "Stop Impersonation"

## Maintenance
See [Maintenance from owner image](https://github.com/sameersbn/docker-gitlab#maintenance)

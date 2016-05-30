# CI Tools Demo

This GitHub repository contains Dockerfiles for running a set of Continuous Integration Tools with a single command.

## Getting started

To get all docker containers running, clone this repo, open docker-compose.yml and edit password + emails, then run:

```
# cd docker-ci-tool-stack
# docker-compose up
```

> Make sure that the host as a docker group with the same GID has in jenkins/Dockerfile (eg: 999). This will allow jenkins to run docker containers on the host

## Configuring

- [Gitlab](doc/GITLAB.md)
- [Jenkins](doc/JENKINS.md)
- [Sonar](doc/SONAR.md)

Here is an overview of all tools:

- GitLab is used for storing the Source Code
- Jenkins contains build job and is triggered once projects in GitLab are updated
- As part of the CI build, Jenkins triggers a static code analysis and the results are stored in SonarQube

## TODO
- Add the ELK stack to monitor logs on all services / containers
- Add Selenium Hub
- Add a Docker Registry

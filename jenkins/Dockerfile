FROM jenkins:1.651.1

USER root

#
# Create a docker group with same GID as Host and add it to jenkins user
#
RUN groupadd -g 999 docker
RUN usermod -a -G docker jenkins

#
# Install necessary packages
#
RUN apt-get update \
      && apt-get install -y sudo docker.io lxc \
      && rm -rf /var/lib/apt/lists/*

# Add jenkins to sudoer
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

# Install docker-compose
RUN curl -L https://github.com/docker/compose/releases/download/1.7.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

#
# Give the permissions to docker binaries for jenkis
#
RUN chown root:docker /usr/bin/docker
RUN chown root:docker /usr/local/bin/docker-compose
RUN chmod +x /usr/bin/docker
RUN chmod +x /usr/local/bin/docker-compose

#
# Install initial plugins
#
USER jenkins
COPY plugins.txt /var/jenkins_home/plugins.txt
RUN /usr/local/bin/plugins.sh /var/jenkins_home/plugins.txt

#
# Configure Jenkins
#
COPY config/config.xml /usr/share/jenkins/ref/config.xml
COPY config/credentials.xml /usr/share/jenkins/ref/credentials.xml
COPY config/ssh-keys/cd-demo /usr/share/jenkins/ref/.ssh/id_rsa
COPY config/ssh-keys/cd-demo.pub /usr/share/jenkins/ref/.ssh/id_rsa.pub

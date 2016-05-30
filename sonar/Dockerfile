#
# Creates a docker container with SonarQube, incl. several plugins
# Since the original Dockerfile does not support plugins, I
# had to extend the Dockerfile
#
# Original: https://hub.docker.com/_/sonarqube/
#

FROM java:openjdk-8-jdk

MAINTAINER BAILLY Romain <rbailly17000@gmail.com>

ENV SONARQUBE_HOME /opt/sonarqube

# Http port
EXPOSE 9000

# H2 Database port
EXPOSE 9092

# Database configuration
# Defaults to using H2
ENV SONARQUBE_JDBC_USERNAME sonar
ENV SONARQUBE_JDBC_PASSWORD sonar
ENV SONARQUBE_JDBC_URL jdbc:h2:tcp://localhost:9092/sonar

ENV SONAR_VERSION 5.5

ENV SONAR_DOWNLOAD_URL https://sonarsource.bintray.com/Distribution

# pub   2048R/D26468DE 2015-05-25
#      Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
# uid       [ unknown] sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
# sub   2048R/06855C1D 2015-05-25
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE

RUN set -x \
    && cd /opt \
    && curl -o sonarqube.zip -fkSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && curl -o sonarqube.zip.asc -fkSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    && gpg --verify sonarqube.zip.asc \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*

# Installing Plugins
RUN cd /opt/sonarqube/extensions/plugins/ \
  && curl -o sonar-web-plugin-2.4.jar -fkSL $SONAR_DOWNLOAD_URL/sonar-web-plugin/sonar-web-plugin-2.4.jar \
  && curl -o sonar-php-plugin-2.8.jar -fkSL $SONAR_DOWNLOAD_URL/sonar-php-plugin/sonar-php-plugin-2.8.jar \
  && curl -o sonar-javascript-plugin-2.12.jar -fkSL $SONAR_DOWNLOAD_URL/sonar-javascript-plugin/sonar-javascript-plugin-2.12.jar \
  && curl -o sonar-json-plugin-1.4.jar -fkSL https://github.com/racodond/sonar-json-plugin/releases/download/1.4/sonar-json-plugin-1.4.jar \
  && curl -o sonar-css-plugin.jar -fkSL https://github.com/SonarQubeCommunity/sonar-css/releases/download/1.9-RC1/sonar-css-plugin.jar \
  && curl -o qualinsight-plugins-sonarqube-badges-1.2.1.jar -fkSL http://central.maven.org/maven2/com/qualinsight/plugins/sonarqube/qualinsight-plugins-sonarqube-badges/1.2.1/qualinsight-plugins-sonarqube-badges-1.2.1.jar \
  && curl -o sonar-scm-git-plugin-1.1.jar -fkSL http://downloads.sonarsource.com/plugins/org/codehaus/sonar-plugins/sonar-scm-git-plugin/1.1/sonar-scm-git-plugin-1.1.jar

VOLUME ["$SONARQUBE_HOME/data", "$SONARQUBE_HOME/extensions"]

WORKDIR $SONARQUBE_HOME
COPY run.sh $SONARQUBE_HOME/bin/
RUN chmod +x $SONARQUBE_HOME/bin/run.sh

ENTRYPOINT ["./bin/run.sh"]

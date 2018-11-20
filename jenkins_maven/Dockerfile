FROM maven:3.6.0-jdk-8 as maven

FROM jenkins/jenkins

USER root

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

USER jenkins

RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state

COPY --from=maven /usr/share/maven /usr/share/maven

ENV MAVEN_HOME /usr/share/maven
ENV PATH=${MAVEN_HOME}/bin:${PATH}
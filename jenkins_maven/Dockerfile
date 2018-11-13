FROM maven:3.6.0-jdk-8 as maven

FROM jenkins/jenkins

COPY --from=maven /usr/share/maven /usr/share/maven

ENV MAVEN_HOME /usr/share/maven
ENV PATH=${MAVEN_HOME}/bin:${PATH}
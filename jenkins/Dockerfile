FROM jenkins/jenkins

USER root

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

USER jenkins

RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state
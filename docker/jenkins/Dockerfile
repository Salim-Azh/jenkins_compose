FROM jenkins/jenkins:2.176.3-jdk11
USER root
COPY jenkins-plugin-manager*.jar /usr/share/jenkins/ref/jenkins-plugin-manager.jar
COPY plugins.txt /usr/share/jenkins/ref/
RUN apt-get -y update && \
    apt-get -y install git && \
    java -jar /usr/share/jenkins/ref/jenkins-plugin-manager.jar -f /usr/share/jenkins/ref/plugins.txt && \
    rm /usr/share/jenkins/ref/jenkins-plugin-manager.jar /usr/share/jenkins/ref/plugins.txt
USER jenkins
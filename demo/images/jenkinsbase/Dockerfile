# JA Bride:  changed to RHEL7
#FROM centos:centos6
FROM rhel7.1

MAINTAINER Thomas Qvarnstrom <tqvarnst@redhat.com>

USER root
# Update the system
RUN yum -y update; yum clean all

##########################################################
# Install Java JDK and other useful cmdline utilities
##########################################################
RUN yum -y install java-1.7.0-openjdk-devel which unzip sudo openssh-clients iputils iproute httpd-tools wget tar initscripts; yum clean all
ENV JAVA_HOME /usr/lib/jvm/jre

############################################
# Install Jenkins
############################################
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
RUN rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
RUN yum -y install jenkins-1.587

RUN cp /etc/sysconfig/jenkins /etc/sysconfig/jenkins.save
RUN sed -i 's/^JENKINS_ARGS=.*/JENKINS_ARGS="--prefix=\/jenkins"/' /etc/sysconfig/jenkins

############################################
# Install Maven
############################################

RUN wget -O /etc/yum.repos.d/epel-apache-maven.repo http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo
RUN yum -y install apache-maven

############################################
# Install Git
############################################

RUN yum -y install git

############################################
# Configure Jenkins
############################################
# Jenkins settings 
ADD config.xml /var/lib/jenkins/config.xml
RUN mkdir /var/lib/jenkins/.m2
ADD settings.xml /var/lib/jenkins/.m2/settings.xml

# Maven
ADD hudson.tasks.Maven.xml /var/lib/jenkins/hudson.tasks.Maven.xml

# Plugins
RUN mkdir -p /var/lib/jenkins/plugins
RUN wget -O /var/lib/jenkins/plugins/git.hpi http://updates.jenkins-ci.org/download/plugins/git/2.2.4/git.hpi
RUN wget -O /var/lib/jenkins/plugins/git-client.hpi http://updates.jenkins-ci.org/download/plugins/git-client/1.10.1/git-client.hpi
RUN wget -O /var/lib/jenkins/plugins/scm-api.hpi http://updates.jenkins-ci.org/download/plugins/scm-api/0.2/scm-api.hpi

RUN chown -R jenkins:jenkins /var/lib/jenkins
  
RUN sed -i "s/--daemon//g" /etc/init.d/jenkins

CMD ["/etc/init.d/jenkins", "start"]

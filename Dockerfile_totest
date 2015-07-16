
FROM centosbase/centos66
ENV ROLE master
ENV SSH_PASS jenkins123
ENV ENV prod
ENV TERMTAG JENKINS


# Jenkins repo
RUN cd /etc/yum.repos.d/ && \
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo && \
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

 
# Run Update, and install Jenkins
RUN yum clean all && \
yum -y update && \
yum -y install jenkins java-1.7.0-openjdk java-1.7.0-openjdk-devel git && \
export JAVA_HOME=/usr/lib/jvm/jre-1.7.0-openjdk.x86_64 && \
yum clean all && \
rm -fr /var/cache/* && \



# Remove locales other than english
cd /usr/share/locale/ && \
for x in `ls | grep -v -i en | grep -v -i local`;do rm -fr $x; done && \
rm -fr ca* den men wen zen && \
cd /usr/lib/locale && \
localedef --list-archive | grep -v -i ^en | xargs localedef --delete-from-archive && \
mv -f locale-archive locale-archive.tmpl && \
build-locale-archive


# Make SSH Directory, Instruct Jenkins not to prompt for host key verification and set perms
RUN mkdir /var/lib/jenkins/.ssh && \
echo -e "Host *\n\tStrictHostKeyChecking no\n" >> /var/lib/jenkins/.ssh/config


#************************
#* Post Deploy Clean Up *
#************************
RUN mkdir -p /var/cache/jenkins/war && \
mkdir /var/log/jenkins || exit 0 && \
cd /var/cache/jenkins/war && \
jar -xvf /usr/lib/jenkins/jenkins.war && \
chmod a+w ./



# Reset Permissions
RUN chown -R jenkins:jenkins /var/cache/jenkins && \
chown jenkins:jenkins /var/log/jenkins && \
chmod -R 775 /var/cache/jenkins && \
chmod -R 777 /var/log/jenkins && \
chown -R jenkins:jenkins /var/lib/jenkins && \
chown -R jenkins:jenkins /var/lib/jenkins/.ssh && \
chmod -R 0700 /var/lib/jenkins/.ssh && \
chmod -R 0600 /var/lib/jenkins/.ssh/*



#**************************
#*  Config Startup Items  *
#**************************
RUN chmod +x /tmp/.runconfig.sh && \
echo "/tmp/./.runconfig.sh" >> /root/.bashrc && \
echo "service jenkins start" >> /root/.bashrc

CMD /bin/bash

#****************************
#* Expose Applicatoin Ports *
#****************************
# Expose ports to other containers only
EXPOSE 22
EXPOSE 8080
EXPOSE 50000




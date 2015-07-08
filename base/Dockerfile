FROM centos:centos6

RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
RUN rpm install -y openssh-server
RUN rpm -y install net-tools vim-enhanced wget openssh-clients nfs-utils screen yum-utils ntp tar
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config


RUN chkconfig  iptables off
RUN chkconfig ip6tables off

RUN ntpdate pool.ntp.org

RUN echo "service ntpd start" >> ~/.bashrc 
RUN echo "service rsyslog start" >> ~/.bashrc
RUN echo "service crond start" >> ~/.bashrc


RUN yum -y update

RUN rpm -e cracklib-dicts --nodeps && yum -y install cracklib-dicts



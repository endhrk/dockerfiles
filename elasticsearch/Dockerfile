# This Dockerfile builds the environment which contains
#   tomcat7 and elasticsearch
#

from ubuntu:14.04
maintainer Hiroki Endo "endhrk@gmail.com"
BUILDCMD docker build -t endhrk/elasticsearch .
RUNCMD docker run -i -t endhrk/elasticsearch /bin/bash

env DEBIAN_FRONTEND noninteractive

env HOME /root
workdir /root

# Configure timezone
RUN echo "Asia/Tokyo" | tee /etc/timezone ; dpkg-reconfigure --frontend noninteractive tzdata

# Configure the location of apt repository
#run echo "deb http://jp.archive.ubuntu.com/ubuntu/ trusty main restricted\n\
#deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty main restricted\n\
#deb http://jp.archive.ubuntu.com/ubuntu/ trusty-updates main restricted\n\
#deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty-updates main restricted\n\
#deb http://jp.archive.ubuntu.com/ubuntu/ trusty universe\n\
#deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty universe\n\
#deb http://jp.archive.ubuntu.com/ubuntu/ trusty-updates universe\n\
#deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty-updates universe\n\
#deb http://security.ubuntu.com/ubuntu trusty-security main restricted\n\
#deb-src http://security.ubuntu.com/ubuntu trusty-security main restricted\n\
#deb http://security.ubuntu.com/ubuntu trusty-security universe\n\
#deb-src http://security.ubuntu.com/ubuntu trusty-security universe\n\
#deb http://extras.ubuntu.com/ubuntu trusty main\n\
#deb-src http://extras.ubuntu.com/ubuntu trusty main\n" > /etc/apt/sources.list
run apt-get update && apt-get -y upgrade && apt-get clean
run apt-get -y install vim wget

run apt-get -y install software-properties-common && add-apt-repository -y ppa:webupd8team/java && apt-get update
run echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
run echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
run apt-get -y install oracle-java7-installer

run wget -qO - http://packages.elasticsearch.org/GPG-KEY-elasticsearch | sudo apt-key add -
run echo "deb http://packages.elasticsearch.org/elasticsearch/1.3/debian stable main" >> /etc/apt/sources.list && apt-get update && apt-get -y install elasticsearch
run sed -i -e "s/^\(MAX_MAP_COUNT=\)[0-9]*/\1/" /etc/init.d/elasticsearch

run cp /usr/share/elasticsearch/bin/elasticsearch.in.sh /usr/share/elasticsearch/bin/elasticsearch.in.sh.orig
run sed -i -e '$a\JAVA_OPTS="$JAVA_OPTS -Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8000"' /usr/share/elasticsearch/bin/elasticsearch.in.sh

run apt-get -y install openssh-server
run mkdir -p /var/run/sshd
run sed -i 's/^UsePAM yes/UsePAM no/' /etc/ssh/sshd_config
run echo "UseDNS no" >> /etc/ssh/sshd_config
run chmod u+s /usr/bin/sudo

run useradd -m -s /bin/bash sshuser
run echo "sshuser:sshpass" | chpasswd
run usermod -G sudo sshuser
run apt-get -y install language-pack-en

cmd service elasticsearch start && /usr/sbin/sshd -D

# expose for sshd
expose 22
# expose for tomcat
expose 8000 8080
# expose for elasticsearch
expose 9200 9300

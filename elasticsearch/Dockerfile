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

run apt-get -y install colord
run apt-get -y install openjdk-7-jdk

run wget -qO - http://packages.elasticsearch.org/GPG-KEY-elasticsearch | sudo apt-key add -
run echo "deb http://packages.elasticsearch.org/elasticsearch/1.3/debian stable main" >> /etc/apt/sources.list && apt-get update && apt-get -y install elasticsearch
run sed -i -e "s/^\(MAX_MAP_COUNT=\)[0-9]*/\1/" /etc/init.d/elasticsearch

run apt-get -y install tomcat7
run sed -i -e 's/#\(JAVA_OPTS="${JAVA_OPTS} -Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n"\)/\1/' /etc/default/tomcat7

run apt-get -y install patch
add ./tomcat7.patch /tmp/
run patch /etc/init.d/tomcat7 < /tmp/tomcat7.patch

cmd service elasticsearch start && service tomcat7 start && cron -f

# expose for tomcat
expose 8000 8080
# expose for elasticsearch
expose 9200 9300

#
#
#

from ubuntu:14.04
maintainer Hiroki Endo "endhrk@gmail.com"

env DEBIAN_FRONTEND noninteractive

# Configure timezone
RUN echo "Asia/Tokyo" | tee /etc/timezone ; dpkg-reconfigure --frontend noninteractive tzdata

# Configure the location of apt repository
run echo "deb http://jp.archive.ubuntu.com/ubuntu/ trusty main restricted\n\
deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty main restricted\n\
deb http://jp.archive.ubuntu.com/ubuntu/ trusty-updates main restricted\n\
deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty-updates main restricted\n\
deb http://jp.archive.ubuntu.com/ubuntu/ trusty universe\n\
deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty universe\n\
deb http://jp.archive.ubuntu.com/ubuntu/ trusty-updates universe\n\
deb-src http://jp.archive.ubuntu.com/ubuntu/ trusty-updates universe\n\
deb http://security.ubuntu.com/ubuntu trusty-security main restricted\n\
deb-src http://security.ubuntu.com/ubuntu trusty-security main restricted\n\
deb http://security.ubuntu.com/ubuntu trusty-security universe\n\
deb-src http://security.ubuntu.com/ubuntu trusty-security universe\n\
deb http://extras.ubuntu.com/ubuntu trusty main\n\
deb-src http://extras.ubuntu.com/ubuntu trusty main\n" > /etc/apt/sources.list && apt-get update && apt-get -y upgrade

run apt-get -y install vim curl

env HOME /root
workdir /root

env DEBIAN_FRONTEND ""


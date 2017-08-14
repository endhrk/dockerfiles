#
#
#

from ubuntu:16.04
maintainer Hiroki Endo "endhrk@gmail.com"

env DEBIAN_FRONTEND noninteractive

run sed -i -e 's/^\([^#][^\/]*\)\/\/archive.ubuntu.com/\1\/\/jp.archive.ubuntu.com/g' /etc/apt/sources.list
run ln -fs /usr/share/zoneinfo/Japan /etc/localtime && dpkg-reconfigure -f noninteractive tzdata

run apt-get update && apt-get -y upgrade && apt-get -y install vim curl net-tools iputils-ping && apt-get clean

env HOME /root
workdir /root

env DEBIAN_FRONTEND ""


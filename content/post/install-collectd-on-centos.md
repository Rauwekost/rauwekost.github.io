+++
date = "2016-01-17T21:12:00+02:00"
description = "Install script for collectd with riemann on centos"
title = "Install collectd on centos"
tags = ["code", "linux"]

+++
A couple of days ago i had to install and configure collectd on a old centos machine. We used collectd in combination with Riemann, so i had to install protobuf-c as well.
here is a quick script to install the necessary components:

```bash
#!/bin/bash
# Perform installation as root

#you need epel to quickly install protobuf-c (riemann_write)
#sudo rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# Install prereqs
yum -y install libcurl libcurl-devel rrdtool rrdtool-devel rrdtool-prel libgcrypt-devel gcc make gcc-c++ yajl yajl-devel protobuf-c protobuf-c-devel

# Get Collectd, untar it, make it and install
wget http://collectd.org/files/collectd-5.4.0.tar.gz
tar zxvf collectd-5.4.0.tar.gz
cd collectd-5.4.0
./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib --mandir=/usr/share/man --enable-all-plugins
make
make install

# Copy the init.d script
cp /root/collectd-5.4.0/contrib/redhat/init.d-collectd /etc/init.d/collectd

# Set the correct permissions
chmod +x /etc/init.d/collectd

# Start the deamon
service collectd start
```

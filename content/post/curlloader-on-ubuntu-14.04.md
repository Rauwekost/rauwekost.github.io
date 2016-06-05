+++
date = "2015-11-10T22:48:24+02:00"
description = "How to simulate load using curl-loader on ubuntu 14.04"
title = "Curlloader on ubuntu 14.04"
tags = ["code", "linux"]

+++

How to simulate load using curl-loader on ubuntu 14.04

## __Install__
download curl-loader from http://curl-loader.sourceforge.net and <code>scp</code> it to the ubuntu host.

```shell
#install build tools
apt-get install libssl-dev build-essential

#unarchive
bunzip2 curl-loader-0.56.tar.bz2
tar xvf curl-loader-0.56.tar

#make, install
cd curl-loader-0.56
make
make install
```

#### Tune the system
Update some system settings as the quick start describes
```
#file limits
ulimit -n 10000
echo 1 > /proc/sys/net/ipv4/tcp_tw_reuse
```

This completes the installation

## __Using curlloader__

#### Example Configuration
```
########### GENERAL SECTION ################################
CLIENTS_NUM_MAX=100
CLIENTS_NUM_START=2
CLIENTS_RAMPUP_INC=5
INTERFACE=eth0 #(depending on ifconfig)
NETMASK=16

IP_ADDR_MIN= <ifconfig-result>
IP_ADDR_MAX= <ifconfig-result>
IP_SHARED_NUM=100

########### URL SECTION ####################################
URL=http://<your-url>
URL_SHORT_NAME="my-site"
REQUEST_TYPE=GET
TIMER_URL_COMPLETION = 5000
TIMER_AFTER_URL_SLEEP = 500
```

Check the curlloader documentation or example config files for more detailed information.

#### Let's run a test!

```
curl-loader -f <config-file-path>
```

#### Reading the output

After the test completes (or gets cancelled) the following files become available:

- yourname.txt
- yourname.ops
- yourname.log
- yourname.ctx

<br />
The one that gives the most information is the <code>.txt</code> file:

```
RunTime(sec),Appl,Clients,Req,1xx,2xx,3xx,4xx,5xx,Err,T-Err,D,D-2xx,Ti,To
0, H/F   , 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
0, H/F/S , 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
3, H/F   , 11, 25, 0, 14, 0, 0, 0, 0, 0, 829, 829, 177426, 1528
3, H/F/S , 11, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
6, H/F   , 26, 33, 0, 19, 0, 0, 0, 0, 0, 1722, 1722, 208880, 2012
6, H/F/S , 26, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
9, H/F   , 41, 39, 0, 23, 0, 0, 0, 0, 0, 3002, 3002, 285667, 2386
9, H/F/S , 41, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
12, H/F   , 50, 27, 0, 18, 0, 0, 0, 0, 0, 4116, 4116, 204675, 1650
12, H/F/S , 50, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
15, H/F   , 50, 13, 0, 13, 0, 0, 0, 0, 0, 5670, 5670, 179276, 798
*, *, *, *, *, *, *, *, *, *, *, *, *, *
RunTime(sec),Appl,Clients,Req,1xx,2xx,3xx,4xx,5xx,Err,T-Err,D,D-2xx,Ti,To
48, H/F   , 50, 359, 0, 252, 0, 0, 57, 0, 0, 6302, 5465, 195592, 1373
48, H/F/S , 50, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
```

#### Output explination
* run-time in seconds;
* List item
* requests num;
* 1xx success num;
* 2xx success num;
* 3xx redirects num;
* client 4xx errors num;
* server 5xx errors num;
* other errors num, like resolving, tcp-connect, server closing or empty responses number (Err);
* url completion time expiration errors (T-Err);
* average application server Delay (msec), estimated as the time between HTTP request and HTTP response without taking into the account network latency (RTT) (D);
* average application server Delay for 2xx (success) HTTP-responses, as above, but only for 2xx responses. The motivation for that is that 3xx redirections and 5xx server errors/rejects may not necessarily provide a true indication of a testing server working functionality (D-2xx);
* throughput in, batch average, Bytes/sec (T-In);
* throughput out, batch average, Bytes/sec (T-Out);

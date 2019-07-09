---
title: "Golang WebApp Upstart Service"
date: 2014-10-07T20:24:04+05:30
comments: true
categories: programming, golang
---

I am running a small webapp and wanted a small service to restart the webapp on boot. There are lot of scripts and tutorials out there, but the solution turned out to be pretty simple.

the webapp binary is upn. 

``` bash file &#47;etc/init/upn.conf
# upnworld - upn job file

description "upnworld.com  server"
author "Darshan Sonde<darshan.sonde@gmail.com>"

start on starting mysql
stop on shutdown

chdir /home/darshansonde/go/bin
exec /home/darshansonde/go/bin/upn 2>&1 >/var/log/upn.log
```

to start service 

``` bash
initctl start upn
```

to stop service

``` bash
initctl stop upn
```

to restart service

``` bash
initctl restart upn
```

## References

1. [Upstart Cookbook](http://upstart.ubuntu.com/cookbook/)
2. [Upstart Getting Started](http://upstart.ubuntu.com/getting-started.html)

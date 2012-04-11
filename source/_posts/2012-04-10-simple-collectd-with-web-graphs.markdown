---
layout: post
title: "Simple collectd with Web Graphs"
date: 2012-04-10 19:04
comments: true
categories: [collectd, lighttpd, cgp]
---
I want to collect machine metrics and I want to display them as a
pretty set of graphs through a web browser.  Here's how I do it:

1. Install [collectd](http://collectd.org) - this gathers the metrics.
2. Install [lighttpd](http://lighttpd.net) - my web server preference,
   also install php.
3. Download and install
   [Collectd Graph Panel](http://pommi.nethuis.nl/category/cgp/), a PHP
   webapp. 

Thats the short version!

<!-- more -->

## Installation

The following walk through will demonstrate the installation using
Ubuntu Oneiric (11.10).  Note, the `sudo` is generally required, but
dropped from the commands listed below:
```
apt-get install collectd lighttpd php5-cgi php5
```
next, edit `/etc/php5/cgi/php.ini` to enable php5 in lighttpd and
uncomment the line `cgi.fix_pathinfo=1`
```
lighttpd-enable-mod fastcgi
lighttpd-enable-mod fastcgi-php
service lighttpd force-reload
```
Now download and '_install_' Collectd Graph Panel(CGP).  CGP is
downloaded with '[git](http://git-scm.com)', if you do not have git
installed then `apt-get install git`
```
cd /var/www
git clone http://git.nethuis.nl/pub/cgp.git
```
 
Done.  Browse to `http://localhost/cgp/` to access the graphs.

## Configuring what metrics collectd collects

The primary configuration file for [collectd](http://collectd.org) is
installed in `/etc/collectd/collectd.conf`, under Ubuntu.
The configuration file is commented; refer to the collectd
'[Documentation](http://collectd.org/documentation.shtml)' page for
more information.  For each plugin see the plugin pages listed along the side of the
documentation page.  Many plugins do not require additional
configuration; simply uncommenting them is a good starting point.

After editing the collectd configuration, the service must be
restarted with the typical service restart command, `service collectd
restart`. 

Many of the plugins default to collecting all possible metrics covered
by the plugin.  It is possible, via the plugin's specific
configuration, to prune out unwanted metrics.  A good example of this
is the 'interfaces' plugin which includes statistics on the loopback
interface by default.

## Customizing Collectd Graph Panel

The [Collectd Graph Panel(CGP)](http://pommi.nethuis.nl/category/cgp/)
website does not provide any configuration documentation, however it
is not difficult to customize CGP.  If installed, as show above, CGP's
configuration file will be `/var/www/cgp/conf/config.php`.  This file
is commented with details for changing the configuration.

## Adding additional hosts

Nothing is required of CGP to add additional hosts.  Simply install
'collectd' on the additional machines and configure the
[network plugin](http://collectd.org/wiki/index.php/Plugin:Network) on
both the client and server.  In most cases, the commented out
configuration, which uses multicast, will suffice for configuring the
network plugin. 

Now sit back, and enjoy the pretty graphs of your running servers.

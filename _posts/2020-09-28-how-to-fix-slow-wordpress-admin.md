---
layout: post
title: "How to fix slow WordPress admin!"
description: "Possible issues, improvements, and fixes to improve WordPress admin performance"
tags: wordpress performance
---

One unusual causing factor that is rarely considered or even caught, are the 
*inefficient*/*unnecessary* **API calls from within the installed and active plugins.**

**Note:** If you need support on these types of matters, don't hesitate to [contact me](/contact).

But before we look into that, let's have a look at a brief list of potential improvements.

## Other WordPress fixes and improvements
These will have a direct correlation to security and performance improvements

* **Cut down the use of active plugins**
    * Remove the plugins that sit there and accumulate dust, getting usage once a year.
    They'll be available on the Plugins directory when you need them again.
    * Fewer plugins mean fewer things to process, potential conflicts, and performance hoggers.
* **Update LAMP/LEMP server** *(if you have control/access to the server hosting your site)*
    * First, take a snapshot of the server for backup purposes.
    * Then have a look at the available packages to update. `yum update`, and `apt update && apt upgrade` are the usual commands
    * The main targets are PHP, Apache/Nginx, and MySQL/MariaDB, but don't omit other packages if they come up
    * Update your packages gradually (i.e: security patches first, then libraries, then services, etc.)
* **Upgrade to PHP7**
    * This in case you're still running on the old and discontinued version PHP 5.x
* **Run your DB server elsewhere**
    * Databases are memory intensive and can pose possible security, performance and service availability problem.
    * My solution is to use a managed DB service, to remove management overhead, and have
    easy access to enabling high availability.

## Inefficiency of plugins
Now, for the less obvious culprit... badly written plugins, hogging resources.

It happens that you need a plugin to provide certain functionality, but few WordPress plugins (unfortunately)
are designed efficiently and well built.

What's more frustrating though, is when you reach out to the developer to explain the issue and show where it happens,
only to get a *"It works well for me"* kind of reply.

In my case, this issue came up whilst using the *Modern Events Calendar (MEC)* by Webnus.
With and thanks to the [Query Monitor](https://querymonitor.com/), I saw that the plugin accounted for:
* the horribly slow loading of certain parts of the WordPress admin panel
* 28 out of 31 HTTP calls (over 90%)
* an average of 16s extra of loading time (over 95% out of all the loading time)

![MEC HTTP Calls](/assets/images/mec-inefficiency.png "MEC HTTP Calls")

### Solution

Whilst the plugin could optimize their calls, and perhaps do it different rather than on every call and successively for every add-on

Since I already paid for the PRO version and verified the license, I figured out that I can block the calls to the following endpoints:
* `http://webnus.biz/webnus.net/plugin-api/verify`
* `http://webnus.biz/webnus.net/addons-api/verify`

Have a look at my article on how to [block outgoing URL calls with iptables]({% post_url 2021-10-26-block-outgoing-url-calls-with-iptables %}) to see how I fixed this issue cause by the plugin
---
layout: post
title: "How to fix slow WordPress admin?"
categories: wordpress performance slow iptables
---

One unusual causing factor that is rarely considered or even caught,
are countless "stupid" API calls from plugins to check for License validity or possible Updates.

**Note:** If you need support on these type of matters, don't hesitate to [contact me](/contact).

## Other possible issues
Here are some other considerations to keep in mind, to improve your WordPress performance overall:
* *Cut down the use of active plugins.*
    * Remove those plugins that sit there and accumulate dust, getting once a year usage.
    They'll be available on the Plugins directory when you need them again.
    * Less plugins mean less things to process, potential conflicts and performance hoggers.
* *Update your server*.
    * This is if you have control/access to the server hosting your site.
    * First, take a snapshot of the server for backup purposes.
    * Then have a look at the available packages for update: `yum update`, and `apt update && apt upgrade` are the usual commands
    * The main targets are PHP, Apache/Nginx and MySQL/MariaDB, but don't omit other packages if they come up
    * Update your packages gradually (i.e: security patches first, then libraries, then services, etc.)
* *Upgrade to PHP7*
    * This in case you're still running on the old and discontinued version PHP 5.x
* *Run your DB server elsewhere*
    * Databases are memory intensive, and can pose a possible security, performance and service availability problem.
    * My go to solution is to use a managed DB service where I remove management overhead and have
    easy access to enabling high availability.

## Inefficiency of plugins
Now, for the less obvious culprit... badly written plugins hogging resources.

It happens that you need a plugin to provide a certain functionality, but few WordPress plugins (unfortunately)
are designed efficiently and well built (not easy task on Wordpress to be honest).

What's more frustrating though, is when you reach out to the developer to explain the issue and showing where it happens,
only to get a *"It works well for me"* kind of reply.

In my case, this issue came up whilst implementing the *Modern Events Calendar (MEC)* by Webnus, which I figured out
that IT was the issue, thanks to the [Query Monitor](https://querymonitor.com/) plugin whom showed that the plugin accounted for:
* the horribly slow loading of the Plugins or Updates sections
* 28 out of 31 HTTP calls (over 90%)
* an average of 16s extra of loading time (over 95% out of all the loading time)

![MEC HTTP Calls](/assets/images/mec-inefficiency.png "MEC HTTP Calls")

## Solution

Since I already paid for the PRO version and verified the license, I figured out that I can block the calls to the following endpoints:
* `http://webnus.biz/webnus.net/plugin-api/verify`
* `http://webnus.biz/webnus.net/addons-api/verify`

For this you'll need a kernel compiled with Netfilter ["String match support"](https://unix.stackexchange.com/questions/404482/) enabled.

```
iptables -A OUTPUT -p tcp -m string --string "/webnus.net/plugin-api/verify" --algo kmp -j REJECT --reject-with tcp-reset
iptables -A OUTPUT -p tcp -m string --string "/webnus.net/addons-api/verify" --algo kmp -j REJECT --reject-with tcp-reset
```

* `-A OUTPUT` Appends a rule targeting the outgoing calls
* `-p tcp` Use the *tcp* protocol for the rule to do its checks
* `-m string --string "PATTERN" --algo kmp`
    * `-m string` Use the *match* module with the *string* selector
    * `--string "PATTERN"` Match the given pattern.
    * `--algo kmp` Use the *KMP* algorithm to do the string matching. Can read more about it here
        * https://stackoverflow.com/questions/16085201/when-would-you-use-kmp-over-boyer-moore
        * https://stackoverflow.com/questions/12656160/what-are-the-main-differences-between-the-knuth-morris-pratt-and-boyer-moore-sea
* `-j REJECT --reject-with tcp-reset` Connection reset: instead of dropping the packet with -j DROP, we can reject it and immediately close the connection with -p tcp -j REJECT --reject-with tcp-reset.

Alternatively and for extra cookie points, you can always block all outgoing connections and whitelist
only the ones are ok with you.

Have a read [here](https://askubuntu.com/questions/909984/how-to-fix-iptables-if-i-have-blocked-all-incoming-and-outgoing-connections) if you want to know more.

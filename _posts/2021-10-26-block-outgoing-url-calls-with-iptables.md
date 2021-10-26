---
layout: post
title: "How to block outgoing URL calls with iptables on Linux"
description: "Using iptables to block outgoing URL calls"
tags: linux iptables 
---

`iptables` is a command-line firewall utility that uses policy chains to allow or block traffic. When a connection tries to establish itself on your system, iptables looks for a rule in its list to match it to. If it doesn't find one, it resorts to the default action.

For this in particular, you'll need a kernel compiled with Netfilter ["String match support"](https://unix.stackexchange.com/questions/404482/) enabled.

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

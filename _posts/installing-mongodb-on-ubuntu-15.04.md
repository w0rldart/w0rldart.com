A few weeks ago, I had updated Ubuntu on a laptop of mine, to 15.10\. When I tried to install and configure [MongoDB](http://mongodb.com), I had completely overseen the fact that with the upgrade from Ubuntu 14.04 (Trusty) to 15.04 (Vivid),  [Upstart](http://upstart.ubuntu.com/) had been replaced with [systemd](http://freedesktop.org/wiki/Software/systemd/). That has caused me a few brainscratches of why mongo is not working as a service. Have a look through the [Ubuntu (Vivid) 15.04 release notes](https://wiki.ubuntu.com/VividVervet/ReleaseNotes)  

# The issue

Few things that you'll notice when you're installing MongoDB ([Install steps](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/#import-the-public-key-used-by-the-package-management-system)), are:

*   Whilst installing, you're see first clue of things going wrong when message shows:

<pre class="EnlighterJSRAW" data-enlighter-language="null">invoke-rc.d: mongod.service doesn't exist but the upstart job does. Nothing to start or stop until a systemd or init job is present.</pre>

*   Executing service to see it's status, will just confirm the above `service mongod status` outputs

<pre class="EnlighterJSRAW" data-enlighter-language="null">● mongod.service
Loaded: not-found (Reason: No such file or directory)
Active: failed (Result: exit-code) since Tue 2016-01-12 17:37:43 GMT; 6min ago

.....</pre>

*   there is no **init.d** file for **mongod**

# Tried this, doesn't work

First thing I tried, was to look for an **init.d** script on [mongo's official github repository](https://github.com/mongodb/mongo/), and I had found it here: [https://github.com/mongodb/mongo/blob/master/debian/init.d](https://github.com/mongodb/mongo/blob/master/debian/init.d). After downloading it and setting it with execution permissions, executing wouldn't bring any joy.

<pre class="EnlighterJSRAW" data-enlighter-language="null">cd /etc/init.d

wget https://raw.githubusercontent.com/mongodb/mongo/master/debian/init.d -O mongod

chmod +x mongod

./mongod start</pre>

<span style="line-height: 1.75em;">resulting in</span>

<pre class="EnlighterJSRAW" data-enlighter-language="null">[....] Starting mongod (via systemctl): mongod.serviceFailed to start mongod.service: Unit mongod.service failed to load: No such file or directory.
failed!</pre>

# The actual solution

For that, I've put together an install script that could handle it all for you. https://gist.github.com/w0rldart/21b9b8544fa7b6fbf0e2 Enjoy!

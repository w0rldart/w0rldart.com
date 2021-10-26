---
layout: post
title: "Installing MongoDB on Ubuntu 15.04"
description: ""
date: 2016-01-13 19:23:01 -0600
tags: ubuntu mongodb linux
---

Starting with ubuntu 15.04, [Upstart](http://upstart.ubuntu.com/) has been replaced with [systemd](http://freedesktop.org/wiki/Software/systemd/), and whilst installing [MongoDB](http://mongodb.com) was no issue, there was no way of having it running as a service by default.

First clue comes when installing MongoDB ([Install steps](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/#import-the-public-key-used-by-the-package-management-system)) with the following message:
```
invoke-rc.d: mongod.service doesn't exist but the upstart job does. Nothing to start or stop until a systemd or init job is present.
```

And indeed `service mongod status` confirms that the service is not running
```shell
● mongod.service
Loaded: not-found (Reason: No such file or directory)
Active: failed (Result: exit-code) since Tue 2016-01-12 17:37:43 GMT; 6min ago
.....
```

and also, there is no **init.d** file for **mongod**

## Tried this, doesn't work
Tried adding back the **init.d** script which I had found [here](https://github.com/mongodb/mongo/blob/master/debian/init.d), but after installing it, there was still no joy.

```shell
cd /etc/init.d

wget https://raw.githubusercontent.com/mongodb/mongo/master/debian/init.d -O mongod

chmod +x mongod

./mongod start
```

resulting in

```
[....] Starting mongod (via systemctl): mongod.serviceFailed to start mongod.service: Unit mongod.service failed to load: No such file or directory.
failed!
```

## Final solution
I've put together an install script that could handle it all for you
<script src="https://gist.github.com/w0rldart/21b9b8544fa7b6fbf0e2.js"></script>

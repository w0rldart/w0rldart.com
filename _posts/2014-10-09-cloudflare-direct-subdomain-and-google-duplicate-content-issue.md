---
layout: post
title: "Using a 'direct' subdomain at CloudFlare and avoiding Google duplicate content penalties"
date: 2014-10-09 13:30:01 -0600
categories: web dns cdn google
---

Don't know how many of you had realized this problem so far, but having a **direct** sub domain at Cloudflare, for example: **direct.domain.com**, which has the purpose to serve a live non-cached of the site, can create some duplicate content issues at google, and that's never good. Well, the best and easiest solution that I found, was to redirect `robots.txt` to a `robots.php` **Apache Rewrite rule**

{% highlight apache %}
# Server robots.txt from the script, to fix the possible duplicate content on google,
# regarding the direct. domain used with cloudflare, used to get a non-cached view.
RewriteCond %{REQUEST_URI} robots\.txt$ [NC]
RewriteRule .* /robots.php [L]
{% endhighlight %}

**<span style="line-height: 1.75em;">Nginx vhost directive</span>**

{% highlight nginx %}
rewrite ^/robots.txt /robots.php last;
{% endhighlight %}

  And in `robots.php` having

{% highlight php %}
<?php

header('Content-type: text/plain');

if ($_SERVER['HTTP_HOST'] == 'direct.domain.com') {
    echo "User-agent: *\n";
    echo "Disallow: /\n";
} else {
    include('robots.txt');
}
{% endhighlight %}

This allowed me to block all robots, whenever they tried to access the direct subdomain, and that way, avoiding any duplicate content penalties.

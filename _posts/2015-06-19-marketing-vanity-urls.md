---
layout: post
title: "Marketing vanity URLs"
date:  2015-06-19 11:57:01 -0600
categories: marketing web
---

Vanity urls are simple but incredibly efficient redirects to help the marketing teams track their  campaigns via Google Analytics, in this case, and help build reports or track their effectiveness. This is a small walk-through on:

*   How to track traffic to websites from printed leaflets and posters
*   How to use Google Analytics to asses printed marketing campaigns
*   How to create vanity urls
*   How to check ROI (Return on investment) for printed adverts

* * *

**Requirements**

*   Domain(s) that you want to use
*   Server with Apache (can use any other service)
    *   I use [Digital Ocean](https://m.do.co/c/d60d93080803) to host my bespoke servers.
*   Manually register the virtual hosts for each domain you want to process, or via a tool like Plesk or cPanel (under parked domain section) if you have a managed server.

Say that you plan to buy a vanity domain called **icecream2015.com** and you'll want the base redirect to be to **2015.icecream.com.** After pointing the domain's dns to your server, and creating vhost for it (in cPannel, you'd add it as a parked domain), you proceed to add the following **RewriteRule **that will redirect **www.icecream2015.com** and/or **icecream2015.com **to your live site, which in this example will say that is **2015.icecream.com**

{% highlight apache %}
RewriteCond %{HTTP_HOST} ^(www\.)?icecream2015\.com$ [NC]
RewriteRule ^/?$ "http\:\/\/2015\.icecream\.com" [R=301,L]
{% endhighlight %}

<span style="line-height: 1.75em;">After that, I assume that you'd want to proceed onto creating those vanity urls, that will be unique for everything that you want to track. Say that</span> **icecream2015.com/cornilla **<span style="line-height: 1.75em;">would be a link to track an email campaign for a Cornetto (no relation) vanilla ice cream. Well, using a</span> [Google's URL Builder tool](https://ga-dev-tools.appspot.com/campaign-url-builder/)<span style="line-height: 1.75em;"> we can easily create our campaign URLS, for example: for the link above, it would redirect to </span>**2015.icecream.com/catalog/cornetto/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour** Only thing remaining, is to write the rule:

{% highlight apache %}
RewriteCond %{HTTP_HOST} ^(www\.)?icecream2015\.com$ [NC]
RewriteCond %{THE_REQUEST} /tm
RewriteRule . "http:\/\/2015\.icecream\.com\/cornetto\/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour" [R=301,L]
{% endhighlight %}

<span style="line-height: 1.75em;">And voila! When anybody would access</span> **www.icecream2015.com/cornilla** <span style="line-height: 1.75em;">or</span>**icecream2015.com/cornilla**<span style="line-height: 1.75em;">, they would get redirected to </span>**2015.icecream.com/catalog/cornetto/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour** Of course a Google Analytics campaign code is automatically connected to all of the goals you set up in GA, whether they are an item sale, even registration, form completion, etc - so you can see what effect your printed marketing (which has your easy-to-remember web addresses on) has on your business targets, whether they are sales or information based. Remember though that not all people will add the 'vanity' part of the URL, so these campaigns will be indicative rather than perfect. **Update** Fixed link to point to Google's new Campaign URL Builder. Thanks to Prateek for pointing this out; go and checkout his detailed tutorial on how to use the tool at [https://prateekagarwal.com/google-url-builder/](https://prateekagarwal.com/google-url-builder/)

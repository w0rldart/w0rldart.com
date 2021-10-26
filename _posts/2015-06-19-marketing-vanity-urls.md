---
layout: post
title: "Marketing vanity URLs"
tags: marketing seo apache
---

Vanity URLs are simple but incredibly efficient redirects to help track marketing campaigns via Google Analytics, in this case, and help build reports or track their effectiveness.

This is a small walk-through on:

- How to track traffic to websites from printed leaflets and posters
- How to use Google Analytics to asses printed marketing campaigns
- How to create vanity urls
- How to check ROI (Return on investment) for printed adverts

## Requirements

- Domain(s) that you want to use
- Server with Apache (or any other web server)
    - I run my servers on [DigitalOcean](https://m.do.co/c/d60d93080803) and *\* Signing up to DigitalOcean via this link, you receive a $50, 30-day credit as soon as you add a valid payment method to your account.*
- Manually register the virtual hosts for each domain you want to process, or via a tool like Plesk or cPanel (under parked domain section) if you have a managed server.

## Process

Say that you plan to buy a vanity domain called `icecream2015.com` and it needs to redirect to `2015.icecream.com`

After pointing the domain's dns to your server, and creating its vhost (in cPannel, you'd add it as a parked domain), you proceed to add the following
`RewriteRule` that will redirect `www.icecream2015.com` and/or `icecream2015.com` to your live site, which in this case is `2015.icecream.com`

```apache
RewriteCond %{HTTP_HOST} ^(www\.)?icecream2015\.com$ [NC]
RewriteRule ^/?$ "http\:\/\/2015\.icecream\.com" [R=301,L]
```

Now, in order to create vanity urls (i.e: `icecream2015.com/cornilla` which tracks an email campaign) you can use [Google's URL Builder tool](https://ga-dev-tools.appspot.com/campaign-url-builder/)
to create our campaign URLS. For example, setting `icecream2015.com/cornilla` to redirect to `2015.icecream.com/catalog/cornetto/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour`

Only thing remaining, is to write the rule:

```apache
RewriteCond %{HTTP_HOST} ^(www\.)?icecream2015\.com$ [NC]
RewriteCond %{THE_REQUEST} /tm
RewriteRule . "http:\/\/2015\.icecream\.com\/cornetto\/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour" [R=301,L]
```

And voila! When anybody goes to `www.icecream2015.com/cornilla` or `icecream2015.com/cornilla`, they would get redirected to `2015.icecream.com/catalog/cornetto/vanilla?utm_source=Cornetto&utm_medium=email&utm_campaign=vanilla_flavour`

Of course a Google Analytics campaign code is automatically connected to all of the goals you set up in GA, whether they are an item sale,
event registration, form completion, etc - so you can track the effectiveness of your campaign (which now is via an easy-to-remember url).

Remember though that not all people will add the 'vanity' part of the URL, so these campaigns will be indicative rather than perfect.

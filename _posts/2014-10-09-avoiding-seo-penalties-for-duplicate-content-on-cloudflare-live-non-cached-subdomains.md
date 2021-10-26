---
layout: post
title: "Avoiding SEO penalties for duplicate content on Cloudflare live/non-cached subdomains"
tags: cloudflare robots.txt dns cdn google seo
redirect_from:
  - /cloudflare-direct-subdomain-and-google-duplicate-content-issue
---

Having a DNS entry on Cloudflare, for example: **direct.domain.com**, which has the purpose to
serve a live and non-cached version of the site, can create some duplicate content issues with Google,
which is never good for SEO.

Best and easiest solution that I found to date, is to redirect `robots.txt` to a `robots.php` via a **rewrite rule**

Such as

- Apache
```apache
# Server robots.txt from the script, to fix the possible duplicate content on google,
# regarding the direct. domain used with cloudflare, used to get a non-cached view.
RewriteCond %{REQUEST_URI} robots\.txt$ [NC]
RewriteRule .* /robots.php [L]
```

- Nginx
```nginx
rewrite ^/robots.txt /robots.php last;
```

And then `robots.php` does the following

```php
<?php

header('Content-type: text/plain');

if ($_SERVER['HTTP_HOST'] == 'direct.domain.com') {
  echo "User-agent: *\n";
  echo "Disallow: /\n";
} else {
  include('robots.txt');
}
```

This allowed me to block all robots whenever they tried to access the direct subdomain,
and that way, avoiding any duplicate content *SEO penalties*.

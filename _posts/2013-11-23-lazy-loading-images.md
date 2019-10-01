---
layout: post
title: "Lazy loading images with JavaScript"
date: 2013-11-23 18:56:01 -0600
categories: javascript web
---

During the rebuild of my [Videouri](http://videouri.com) project, I was constantly looking for the best approach of writing and serving all the modules involved, and onto that matter, I decided to do a bit of investigation into implementing a lazy load for the images. First thing that came into my head was to server the images, upon DOM ready with the help of jQuery:

{% highlight javascript %}
$(function(){
    $('img.to-load').each(function(index, object) {
        $(this).attr('src', $(this).data('src'));
    });
});
{% endhighlight %}

It's a partial solution, that is better than nothing. But I decided to investigate a little bit, and see what other people come up with for the same goal, or what suggestions they had. What I found, is that there are both server side and client side solutions.

*   Google's [PageSpeed](https://developers.google.com/speed/pagespeed/module "PageSpeed") module:
    *   **URL:** [https://developers.google.com/speed/pagespeed/module/filter-lazyload-images](https://developers.google.com/speed/pagespeed/module/filter-lazyload-images)
    *   Well, we all at least heard of this module, that is currently available for Apache and Nginx, but it seems that the great job they did at Google, it goes even further as they built a lazy load module within it. This is awesome! **BUT, **The problem is that if you run Nginx (like I do) and don't have already Nginx compiled with support for Page Speed module, you'll have to uninstall current instance of Nginx and build it again, which requires time and server downtime, which not many people can afford to do so... unless you're using Tengine. [https://github.com/pagespeed/ngx_pagespeed#how-to-build](https://github.com/pagespeed/ngx_pagespeed#how-to-build)
    *   It would've been great to manage this lazy loading feature from within Nginx, and avoid loading more js code into the platform, but that is not the proper choice for now.
*   LazyLoad by [tuupola](https://github.com/tuupola)
    *   **URL:** [https://github.com/tuupola/jquery_lazyload](https://github.com/tuupola/jquery_lazyload)
    *   Searching the web, and a little bit with [bower](http://bower.io/), I have found this library that is still maintained, which is great. Not only that is serves the purpose, but it also comes with solutions to common problems when trying to optimize image loading for different devices, so, GREAT!
*   Unveil by [luis-almeida](https://github.com/luis-almeida)
    *   **URL:** [https://github.com/luis-almeida/unveil](https://github.com/luis-almeida/unveil)
    *   This is a light weight version of the library mentioned above.
*   LazyLoad by [vvo](https://github.com/vvo)
    *   **URL: **[https://github.com/vvo/lazyload](https://github.com/vvo/lazyload)
    *   Quote: _Lazyload images, iframes, widgets with a standalone JavaScript lazyloader ~1kb_
    *   Well, this is a good alternative to the ones above, and it seems to go even further by giving lazy load support for not only images, but iframes and widgets as well.

Now you decide what to try, and if you have any comments about this matter, please do share. I personally have implemented the _LazyLoad by [tuupola](https://github.com/tuupola),_ and so far everything is going great.

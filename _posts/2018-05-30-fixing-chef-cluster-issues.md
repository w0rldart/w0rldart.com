---
layout: post
title: "Fixing Chef cluster issues"
categories: chef elasticsearch cluster linux aws
---

Chef is a vital part of the infrastructure for many enterprises, as a configuration management tool to automate
the provisioning of their IT infrastructure, with additional compliance capabilities.

No dispute that it's a great tool, and yes there are many other configuration management tools out there, but
this is not a comparison article, just a *tips and tricks to fix a Chef cluster* from my experience with Chef
accumulated over time.

This article goes hand ind hand with the [High Availability: Backend Cluster](https://docs.chef.io/install_server_ha.html)
setup on [AWS](https://aws.amazon.com/) cloud platform, but be easily translated to any other (like [DigitalOcean](https://m.do.co/c/b3ef1a9e67b9), [Google Cloud](https://cloud.google.com/))

<script src="https://gist.github.com/w0rldart/9747c7ba4b8e8787f285dade4136f91c.js"></script>

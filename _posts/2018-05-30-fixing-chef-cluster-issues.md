---
layout: post
title: "Fixing Chef cluster issues"
description: ""
tags: chef elasticsearch cluster linux aws
---

Chef is a vital part of the infrastructure for many enterprises, as a configuration management tool to automate
the provisioning of their IT infrastructure, with additional compliance capabilities. But in can get tricky when
things stop working and not much specialised information is available.

This is my compilation of *tips and tricks to fix a High Availability Chef cluster* setup on [AWS](https://aws.amazon.com/)
cloud platform, but which can apply to any other cloud vendor (like [DigitalOcean](https://m.do.co/c/b3ef1a9e67b9), [Google Cloud](https://cloud.google.com/))

If you are looking to update your Chef cluster setup on AWS, or implement a new one, I do recommend having a look
at the [AWS Native Chef Server Cluster](https://github.com/chef-customers/aws_native_chef_server).
It was designed and tested for AWS services.

<script src="https://gist.github.com/w0rldart/9747c7ba4b8e8787f285dade4136f91c.js"></script>

---
layout: post
title: "Configuring UTF8 Collation for MySQL or Mariadb on Docker"
categories: docker mysql mariadb linux
---

Configuring UTF8 character-set/collation for your MySQL or MariaDB service on a Docker container
can be achieved by passing the following parameters in the `command` section

 * `--character-set-server=utf8`
 * `--collation-server=utf8_unicode_ci`
 * `--init-connect='SET NAMES UTF8;'`
 * `--innodb-flush-log-at-trx-commit=0`

docker-compose file snippet:
<script src="https://gist.github.com/w0rldart/aa472db45c3817d937a1870a32f77820.js"></script>

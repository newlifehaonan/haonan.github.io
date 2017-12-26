---
layout: post
title: "How to replace Github-page with custom domain"
date: 2017-12-26 2:06:00
tag:
- Git
description: Github pages
comments: true
---
# How to use custom domain on Github page

<hr />

## Set up Step by Step

* Purchase a Domain in one of the following website
  * [Godaddy](https://www.godaddy.com/)

  * [Namecheap](https://www.namecheap.com/")

  * [Bluehost](https://www.bluehost.com/)

  _(Warning :Don't add any extra services like private control, heck prohibition unless your webpage is for business)_

* Write `CNAME` file on your repository _(I recommend write local file and then push to remote)_

  * Write your Personal Domain in the `CNAME` file

* Add Two A records on your domain provider

  * record name : A

  * Host : @

  * point to : 192.30.252.153 and 192.30.252.154

  * TTL : customer

  * second:600

* Reopen your webpage by the new domain name

  _(if 404 not found, wait for 5-10 mins and refresh the url,and you can log in your webpage)_

## Reference URL

*  [How to add a custom domain in Github Pages](https://www.youtube.com/watch?v=hUChaN-VRIc)

* [DNS configuration](https://help.github.com/articles/troubleshooting-custom-domains/#dns-configuration-errors)

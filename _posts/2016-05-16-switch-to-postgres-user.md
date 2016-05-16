---
layout: post
title: Postgres user
description: "how to switch to postgresql user"
modified: 2016-05-16
tags: [Postgresql]
categories: [Postgresql]
share: false
---

I'm documenting this because I always forget. To use psql (the postgresql command line tool) you will first need to log in as the postgres user. There is a nice tutorial [here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04).

To change to the postgres user:

{% highlight shell%}
$ sudo -i -u postgres
{% endhighlight %}

Start psql:

{% highlight shell%}
$ psql
{% endhighlight %}

In one step:

{% highlight shell%}
$ sudo -u postgres psql
{% endhighlight %}
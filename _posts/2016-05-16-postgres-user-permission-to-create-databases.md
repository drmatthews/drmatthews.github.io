---
layout: post
title: Allow postgres user to create database
description: "how to allow a postgres user to create a database"
modified: 2016-05-16
tags: [Postgresql, Django]
categories: [Postgresql]
share: false
---

The context for this is testing in Django, where a test database needs to be created. A permission denied error can occur if the user (not OS user) that owns the database does not have permission to create databases (I created the database and user via psql). Firstly log in as the postgres user (i.e. with the username 'postgres') and then use psql to alter the database ower.

To give the postgres user permission:

{% highlight shell%}
postgres=# ALTER USER username CREATEDB;
{% endhighlight %}

Don't forget the trailing semi-colon.
---
layout: post
title: New Postgres database and user for use in Django
description: "how to create a new postgres database and user for use in django"
modified: 2016-05-24
tags: [Postgresql, Django]
categories: [Postgresql]
share: false
---

Another one I always forget - how to create a new Postgres database and user for a new Django app. This is possible in one step as long as your Postgres configuration allows you to connect as the 'postgres' user:

{% highlight shell%}
$ createdb -Upostgres name-of-database
{% endhighlight %}

Mine does not so I first switch to the 'postgres' user and run the following:

{% highlight shell%}
$ sudo -i -u postgres
{% endhighlight %}

Then use the 'createdb' and 'createuser' commands:

{% highlight shell%}
$ createdb name-of-database
$ createuser -P name-of-user
{% endhighlight %}

Then start psql and grant privileges to the user:

{% highlight shell%}
$ psql
postgres=# GRANT ALL PRIVILEGES ON DATABASE name-of-database TO name-of-user;
{% endhighlight %}
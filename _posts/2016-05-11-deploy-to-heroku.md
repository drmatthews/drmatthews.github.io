---
layout: post
title: Deploying Django app to Heroku
description: "A post about how I deployed a Django app to Heroku"
modified: 2016-05-11
tags: [Django, Heroku, deployment]
categories: [Heroku]
share: false
---

Disclaimer - this isn't necessary the best way, but it is the way that I deployed my Django app on Heroku.

Firstly, the app was built using the [two scoops cookiecutter template](https://github.com/pydanny/cookiecutter-django). This won't be described here - I simply describe the process of deployment.

Heroku provides a [detailed tutorial](https://devcenter.heroku.com/articles/deploying-python) for command line deployment. I am using [Amazon S3](https://aws.amazon.com/s3/) and [Mailgun](https://mailgun.com/app/dashboard) for serving media and emailing respectively. To use these services with Heroku some environment variables need setting and I found this a bit tedious through the command line. Heroku provides a web interface (https://heroku.com/deploy) for deployment where these environment variables can easily be set. To do this your project needs to have an [app.json file](https://devcenter.heroku.com/articles/heroku-button) to specify config variables. Include a template in your project README to include a `deploy` button. So, the process went as follows:

* Go to Github and push the Heroku deploy button on the project README.
* Add app name, define config vars and deploy the app.
* Install the [Heroku toolbelt](https://toolbelt.heroku.com/).
* Use the toolbelt to set a git remote -->

{% highlight shell %}
$ heroku git:remote -a your-app-name
$ Git remote heroku added.
{% endhighlight %}

* This will enable you to easily deploy any changes to Heroku using git -->

{% highlight shell %}
  $ git push heroku master
{% endhighlight %}

* And don't forget to migrate the database -->

{% highlight shell %}
  $ heroku run python manage.py migrate
{% endhighlight %}


---
layout: post
title: Set nav bar item to active using Javascript
description: "A post about using Javascript to set css active on a nav bar item"
modified: 2016-05-11
tags: [Javascript, CSS, nav]
categories: [Javascript]
share: false
---

On a couple of occassions I've found myself needing to set an active selector on a nav bar item using Javascript; this post documents how this was done. I have used this technique in both a straight-forward side-bar nav and on a [Bootstrap](http://getbootstrap.com/) styled nav bar for a Django app.

It is simply done using a bit of [Jquery](https://jquery.com/) and works as follows:

In the case of a simple nav bar (made of `<a>` tags inside `<li>` tags)

{% highlight javascript %}
$(document).ready(function() {
	$(".sidebar [href]").each(function() {
		if (this.href == window.location.href) {
			console.log(this)
		    $(this).addClass("active");
	    }
	});
});
{% endhighlight %}

In the case of a Bootstrap navbar it is also necessary to take care of click events

{% highlight javascript %}

// add 'current' to <li> tag
$('.main-nav ul li a').each(function() {
  if (this.href == window.location.href) {
      $(this).closest('li').addClass('current');
  }
});

// respond to click events
$('.main-nav ul li a').click(function() {
    $('ul li.current').removeClass('current');
    $(this).closest('li').addClass('current');
});         

{% endhighlight%}
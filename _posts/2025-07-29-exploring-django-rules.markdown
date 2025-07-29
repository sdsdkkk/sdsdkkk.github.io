---
layout: post
title: "Exploring Django Rules"
date: 2025-07-29 00:00:00
description: Some notes on the rules package for Django
tags: SoftwareEngineering
---

Recently, I've been working on a hobby project in my free time. There's no real goal yet for this project aside from hacking together something with my sister, [Elvyna](https://elvyna.github.io/), who is a data scientist. Hence, we decided to go with a Python-based project so she can utilize her data skills in this project with support from the Python ecosystem for data analytics and machine learning.

I myself, while quite comfortable working with Python, have mostly used it just for [system scripts and tools](/2022-05-24/tp-link-wireless-router-arp-list-monitoring) development so far. Given that I used to be a Ruby on Rails developer, I decided to go with Django to develop the web application for this project since both Rails and Django were hugely popular back in late 2000s and early 2010s.

One Issue that I found with Django's built-in authorization system is that it was built for a CMS-like application where each user owns certain resources in the system and they may allow other users to access and modify their resources. While this is great for building systems such as news sites where an author can author multiple articles, which they could share with other authors and editors for collaboration (which requires maintaining complex user-to-resource access mapping), this isn't ideal for cases where the access rule is relatively static after its initial rule definition.

Back when using Ruby on Rails, I primarily worked with [CanCan](https://github.com/ryanb/cancan) gem, which was later discontinued and forked as [CanCanCan](https://github.com/CanCanCommunity/cancancan). CanCan and CanCanCan can be used to define access rules in Rails apps in the code itself without writing the rules to the DB, so I was looking for something in Django that works similarly.

I found [django-cancan](https://github.com/pgorecki/django-cancan), which was inspired by the CanCan gem. But after going through it for a bit, I found that django-cancan is still writing the access rules we defined into our DB, which is not something I wanted since I aimed to have the access rule definitions defined just in code and minimize the DB access operations for authorization.

After doing a bit more exploration, I found [django-rules](https://github.com/dfunckt/django-rules). It does exactly what I want, defining the access rules in code without writing the rules to DB and requiring us to read from DB for authorization.

In this post, I'd like to summarize a few things I've learned about how to use django-rules and how I've set it up in our hobby project.

# Setting Up Rules in Project Config

First, install the django-rules package if you haven't.

```
pip install rules
```

In `settings.py`, add the following configurations.

```python
INSTALLED_APPS = [
    # ... some other apps
    'rules.apps.AutodiscoverRulesConfig',
    # ... some other apps
]

AUTHENTICATION_BACKENDS = [
    # ... some other authentication backends
    'rules.permissions.ObjectPermissionBackend',
    # ... some other authentication backends
]
```

For the `INSTALLED_APPS` config, we can simply add `rules` instead of `rules.apps.AutodiscoverRulesConfig` to have the Django server to load `rules.py` from the Django web config directory. But I prefer to have separate `rules.py` files for each Django apps to keep every rule definition localized to the context of its app scope.

In `urls.py`, add the following snippet to enable a custom 403 response error handler.

```python
from django.conf.urls import handler403
from django.shortcuts import render

def custom_403_handler(request, exception):
    return render(request, '403.html', status=403)

handler403 = custom_403_handler
```

This will allow us to set up a custom 403 response page template. In my case, I put it in `templates/403.html` since the Django project is set up to store the view templates in the `templates` directory.

# Defining and Using App Authorization Rules

When you set up a Django app, you can now add a `rules.py` file in the app directory to define the authorization rules.

```python
import rules

# Define custom authorization rules
#
# We can actually use the built-in rules.is_authenticated instead of defining our own rule for
# this, but bear with it since it's just an example snippet
@rules.predicate
def user_authenticated(user):
    return user.is_authenticated

# Register authorization rules
rules.add('site.index', rules.always_allow)
rules.add('site.settings', user_authenticated)
```

The `@rules.predicate` decorator seems to be suggested to be used for our custom rule definitions for django-rules. The source code of the `predicate` implementation of django-rules is available [here](https://github.com/dfunckt/django-rules/blob/master/rules/predicates.py). It basically contains a set of validation sequences to ensure the `predicate` functions are properly implemented, along with a few default `predicate` functions for basic authorization rules.

What I found pretty convenient is the `@permission_required` decorator we can use in our views. The following is a sample `view.py` file.

```python
from django.shortcuts import render
from django.http import HttpResponse
from django.template import loader
from django.core import exceptions
from rules.contrib.views import permission_required

@permission_required('site.index', raise_exception=True)
def index(request):
    template = loader.get_template('site/index.html')
    return HttpResponse(template.render({}, request))
    
@permission_required('site.settings', raise_exception=True)
def settings(request):
    template = loader.get_template('site/settings.html')
    return HttpResponse(template.render({}, request))
```

The `@permission_required` decorator can be used to access the rules we registered in the `rules.py` file and perform a check using the authorization function we put under the registered rule. In this case, we have registered the `site.index` and `site.settings` rules in `rules.py` before, and we're now using it to perform a permission check for executing the `index` and `settings` functions defined in `views.py`.

The `raise_exception=True` parameter is passed to the `@permission_required` decorator to let it throw 403 error response in cases when authorization fails. There are more complex usages of the `@permission_required` decorator as described [here](https://github.com/dfunckt/django-rules?tab=readme-ov-file#permissions-in-views), where we can have the decorator to automatically retrieve the target object the user intends to access to be checked in the authorization rule.

# Conclusion

There are definitely more use cases of the django-rules package that I haven't explored yet, such as calling the authorization logic from model and from view. It also has a bunch of [advanced features](https://github.com/dfunckt/django-rules/tree/master?tab=readme-ov-file#advanced-features) I haven't touched since, in the context of the project I'm currently working on, so far I still haven't felt the necessity for using them (the application is still relatively simple).

But overall, I'm happy to find that there's a Django authorization library where I can simply define the authorization rules in the code without having to touch the DB for storing and reading the permissions. Otherwise, I would have had to write one by myself. While that's definitely doable, it might not be done in the best possible way given that I’m not a Django expert at this point in time.

# References

[Elvyna Tunggawan](https://elvyna.github.io/)

[TP-Link Wireless Router ARP List Monitoring](/2022-05-24/tp-link-wireless-router-arp-list-monitoring)

[ryanb/cancan: Authorization Gem for Ruby on Rails.](https://github.com/ryanb/cancan)

[CanCanCommunity/cancancan: The authorization Gem for Ruby on Rails.](https://github.com/CanCanCommunity/cancancan)

[pgorecki/django-cancan: Authorization library for Django](https://github.com/pgorecki/django-cancan)

[dfunckt/django-rules: Awesome Django authorization, without the database](https://github.com/dfunckt/django-rules)

[django-rules/rules/predicate.py at master](https://github.com/dfunckt/django-rules/blob/master/rules/predicates.py)

---
title:  "Safe Redirect Gem for Rails"
date:   2016-07-23 00:00:00
description: Preventing Open Redirects
---

I've been responding to vulnerability reports at my office since the last quarter of 2015. Open redirects are among the most common reported vulnerabilities for the first few months. Actually, all of the open redirects are reported by the same bug hunter after trying to bypass the filter we put on our application's redirection function again and again.

Our company's main web application is built using Ruby on Rails, so the solution to the open redirect method I built on our site involving sanitization of URLs passed to Rails' `redirect_to` method calls.

The problem with the initial implementation I built was we need to explicitly declare that we want to sanitize the target URLs for redirection on `redirect_to` calls to user-provided parameters as target URLs.

So one Saturday at the end of April 2016 I decided that a better implementation for the filter could be built as its own standalone gem, separated from the company's codebase. I started by listing every possible payload combination for open redirects, and built a plugin so Rails apps can just include and configure the gem to secure their redirects.

The source code and the documentation is available [here](https://github.com/sdsdkkk/safe_redirect), and at this point I consider the gem quite stable.

### Configuring Safe Redirect

Safe Redirect is a tiny gem. To use Safe Redirect on our Rails apps, we need to include this line on our Gemfile.

```rb
gem 'safe_redirect'
```

Add `safe_redirect.rb` on `config/initializers/` directory to configure Safe Redirect. Here's an example configuration.

```rb
SafeRedirect.configure do |config|
  config.domain_whitelists = %w(www.google.com www.twitter.com m.facebook.com)
end
```

The previous configuration will allow redirection to `www.google.com`, `www.twitter.com`, and `m.facebook.com` and block redirection to other domains other than the whitelisted domains if the domain is specified in the target URL.

We can also define the default path to redirect to if the target URL isn't whitelisted.

```rb
SafeRedirect.configure do |config|
  config.domain_whitelists = %w(www.google.com www.twitter.com m.facebook.com)
  config.default_path = '/home'
end
```

By default, the whitelisted domain is set to an empty array and the default redirect path is set to `/`.

### Safely Redirecting

To use Safe Redirect on our application, we need to include Safe Redirect to the controllers we want to secure from open redirects.

```rb
class ApplicationController < ActionController::Base
  include SafeRedirect

  # ... some other code
end
```

Safe Redirect overrides `redirect_to` method from the Rails' ActionController. The following is the implementation in the current version of Safe Redirect (0.2.0).

```rb
def redirect_to(path, options={})
  target = options[:safe] ? path : safe_path(path)
  super target, options
rescue NoMethodError
end
```

With this `redirect_to` method overriding Rails', every redirection to domains other than the whitelisted domains will be filtered unless explicitly stated as safe to redirect when called.

```rb
# Filters the redirect target URL
redirect_to 'https://github.com'

# Skip the filtering and call Rails' redirect_to method as it is
redirect_to 'https://github.com', safe: true
```

### Other Uses

We can also use Safe Redirect to simply check whether an URL is safe according to the whitelist we configured.

```rb
safe_domain?('https://github.com/sdsdkkk')
# => false, github.com is not whitelisted

safe_domain?('https://m.facebook.com')
# => true, m.facebook.com is whitelisted
```

Or to sanitize the URL for purposes other than redirecting.

```rb
safe_path('https://github.com/sdsdkkk')
# => /sdsdkkk

safe_path('https://github.com')
# => /home (default path specified in config file)

safe_path('https:/www.google.com')
# => https://www.google.com
```
---
layout: post
title:  "Hello, Jekyll!"
date:   2015-01-03 11:06:51
categories: jekyll wordpress
---
I have been blogging using [WordPress][WordPress] for several years. I used to write posts on programming and networking as a college student, and since last year I started to blog on the same thing again. Both blogs are using WordPress.

Currently, I'm checking out Jekyll and GitHub pages, as it's more flexible than blogs hosted on WordPress. I had the choice to download WordPress CMS and host it somewhere else myself to get more flexibility. But the security is a bit of a problem, since WordPress is among the top CMSs for script kiddies to hack.

Another thing is I'm not that much into PHP from the beginning, and I kinda hate it now. So maintaining a self-hosted WordPress blog is out of question. Besides right at this years' January 1st, I found out that WordPress doesn't support Haskell in their source code display formatting. My Haskell code snippets turned into plaintext :(

I think I'm a bit late to adapt Jekyll for blogging for a programmer, though. Here's the snippet that's turned into plaintext in [my last WordPress blog post][RandomNumberGuesser].

{% highlight haskell %}
import System.Random
 
tryGuessing :: Int -> IO()
tryGuessing num = do
  putStrLn "Guess a number?"
  guess <- getLine
  if (read guess) < num
    then do 
        putStrLn "Too Low!"
        tryGuessing num
    else if (read guess) > num
      then do
          putStrLn "Too High!"
          tryGuessing num
      else do
          putStrLn "You're awesome!"
 
main = do
  x <- randomRIO (1, 100)
  tryGuessing x
{% endhighlight %}

It's perfectly highlighted here. Woohoo!

Since lots of my posts will be on programming stuff, I think moving to Jekyll might be a good idea. I haven't got the right theme to put and explain snippets of code in WordPress so far. But I think Jekyll's default theme is already good for it.

I think the Jekyll is good since it doesn't require the usage of any DBMS. It might be a bit troublesome to keep track of the posts after some time, since each post need its own separate markdown file. But I'll see about it later.

[WordPress]:             http://wordpress.com
[RandomNumberGuesser]:   http://sdsdkkk.wordpress.com/2015/01/01/haskell-random-number-guesser/
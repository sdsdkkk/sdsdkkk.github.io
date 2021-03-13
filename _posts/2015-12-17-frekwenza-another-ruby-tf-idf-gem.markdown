---
layout: post
title:  "Frekwenza: Another Ruby TF-IDF Gem"
date:   2015-12-17 00:00:00
description: Because I Needed Something Different
tags: SoftwareEngineering
---

One of the personal projects I was working on a few months ago has text processing in it. I needed to classify text messages into several groups depending on what they're talking about.

The problem was there are a lot of noises in the messages, not to mention typos and slangs. I figured that I need to clean the noises from the text before I continue to the processing part, since the noises disrupted the classification.

### Ruby-Tf-Idf

Since I was working with Ruby, I looked for Ruby gems to calculate TF-IDF so I can use that to measure the relevancy of a word in the text and drop the irrelevant ones. I decided to use [Ruby-Tf-Idf](https://github.com/mathieuripert/ruby-tf-idf/) as it's designed with a workflow that covers my need and the documentation is quite clear.

While I think Mathieu Ripert did a good job on Ruby-Tf-Idf (otherwise I wouldn't have used it in my project), I find several things that I'd like to change.

#### 1. Bug on string downcasing

In `split_docs` method, `downcase!` is used instead of `downcase`. This causes the method to raise an error on cases when the downcased string is already using lower case characters, as `downcase!` will return `nil` and `gsub` is called on the `nil` value.

See the code snippet below, taken from Ruby-Tf-Idf's [ruby-tf-idf.rb on line 95-105](https://github.com/mathieuripert/ruby-tf-idf/blob/master/lib/ruby-tf-idf.rb#L100).

~~~rb
def split_docs(docs)

  splitted_docs = []
  docs.each do |d|
    begin
      splitted_docs << d.downcase!.gsub(/,|\.|\'/,'').split(/\s+/)
    rescue
    end
  end
  splitted_docs
end
~~~

The raised error will be rescued by the `rescue` block, but then the value of `splitted_docs` will not be appended. This causes the method to return a wrong result.

#### 2. Hard-coded stop words only supports English and French

I was working with text documents written in Indonesian. It wasn't very convenient since I have to add Indonesian stop words to the source code, rebuild the gem, and replacing the one I installed straight from RubyGems to have Indonesian language support.

If I choose to use the stop words, Ruby-Tf-Idf will proceed to use both the English and French stop words too. So this is how I initialize the object when I don't want to use any stop words.

~~~rb
t = RubyTfIdf::TfIdf.new corpus, limit, false
~~~

And here's when I want to use stop words. I can't specify which set of stop words to use.

~~~rb
t = RubyTfIdf::TfIdf.new corpus, limit, true
~~~

I submitted a [pull request](https://github.com/mathieuripert/ruby-tf-idf/pull/1) for fixing the downcasing bug and adding a list of Indonesian stop words. But as the project goes, I have my third wish.

#### 3. Changing stop words requires modification of gem code

After working on the project for a while, I realized that I have to compare the results when using two different sets of Indonesian stop words. That means I need to keep two versions of Ruby-Tf-Idf gem in my machine, one for the first set of stop words and one for the other set.

I decided that I need to build something that can accept stop words from sources other than hard-coded variables in the gem's code.

### Frekwenza

Not long after I finished the project, I started building [Frekwenza](https://github.com/sdsdkkk/frekwenza) based on Ruby-Tf-Idf's code. I wanted something that's pretty close to Ruby-Tf-Idf, but with some changes those aren't compatible with Ruby-Tf-Idf. So I wrote it under a different project, and gave it a different name.

Starting with the `split_docs` method, the following snippet is the method in Frekwenza.

~~~rb
def split_docs(docs)
  words = []
  docs.each do |d|
     words << d.downcase.gsub(/[^a-z0-9]/, ' ').split(' ')
  end
  words
end
~~~

Besides the switch from `downcase!` to `downcase` method, I also modified the regex used for `gsub`. On Frekwenza, any non-alphanumeric characters will be substituted with a space before the string is split.

And more importantly, with Frekwenza, we can pass the path of a file containing our list of stop words for it to use.

~~~rb
t = Frekwenza::TfIdf.new corpus, limit, "stop_words.txt"
~~~

If we've loaded the file beforehand and we have the list of stop words in the form of an array of string, we can simply pass the array instead of the file location.

~~~rb
t = Frekwenza::TfIdf.new corpus, limit, ["some", "stop", "words"]
~~~

Of course we're free to not use any stop words too, as we can do with Ruby-Tf-Idf.

~~~rb
t = Frekwenza::TfIdf.new corpus, limit
~~~

Since the object initialization is a bit different, it won't work right away if you just switched from Ruby-Tf-Idf to Frekwenza. You need to make several adjustments in the parameters, and you need to keep Ruby-Tf-Idf's hard-coded stop words you've been using somewhere else and pass it to Frekwenza.

### Conclusion

While Ruby-Tf-Idf is quite well-documented, easy to use, and seems to be designed to use in a use-case similar to mine, it doesn't offer a lot of flexibility.

We can take only the relevant part of the source code and modify it for our own uses when we're doing simple experiments. But it's better to have it as a flexible component when using it as a library in a software project.

Think of Frekwenza as an upgrade for Ruby-Tf-Idf that gives better flexibility, yet doesn't provide a good backward compatibility.

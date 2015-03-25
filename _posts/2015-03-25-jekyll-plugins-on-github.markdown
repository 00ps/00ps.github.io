---
layout: post
title: Jekyll plugins on Github
date: 2015-03-25T18:00:00.000Z
categories: jekyll
tags:
  - jekyll
  - git
  - github
  - "tips 'n tricks"
---
##### Intro

[Github][github] allows you to host your website/weblog for free ... that is awesome!
What's great whit Github hosting is that your site IS a git repo, so you benefits from all the git feature (versioning, diff, etc).

Even more awesome! [Github][github] support [Jekyll][jekyll] and generate your web pages on the fly from your markdown files. For those who don't know, [Jekyll][jekyll] is a static site generator that comes with a bunch of [templates][jekyllthemes] and [plugins][jekyll-plugins]...
wait!! plugins!? ... ho no! Github does not support plugins!

No need to panic, you can still generate your site locally and push the html content to Github, this is how:

##### Step by step guide

I found this [interesting howto][howto] which describe the following steps:

1. create a new branch that you are going to use as a working branch, let's call it "source":

{% highlight bash %}
git branch source
{% endhighlight %}

2. add whatever you want to add to your website in this new branch

3. tell jekyll to regenerate the html content of you site:

{% highlight bash %}
jekyll build
{% endhighlight %}

4. once ready to publish, delete the "master" branch:

{% highlight bash %}
git branch -D master
{% endhighlight %}

5. recreate a new "master"

{% highlight bash %}
git checkout -b master
{% endhighlight %}

6. and tell git to filter the content of master:

{% highlight bash %}
git filter-branch --subdirectory-filter _site/ -f
{% endhighlight %}

7. you can finally push your local changes to [Github][github]:

{% highlight bash %}
git push --all origin
{% endhighlight %}

##### Quick explanation

What you've done, is basically telling git to show only the content of the "_site/" directory from the master branch, this is what is going to be published by Github.

##### Troubleshooting

**Found nothing to rewrite**

If git refuses to filter your branch with this message, make sure that you haven't forgotten to remove "_site" from your .gitignore :)

{% highlight bash %}
git filter-branch --subdirectory-filter _site/ -f

Found nothing to rewrite
{% endhighlight %}


[jekyll]: http://jekyllrb.com
[github]: http://www.github.com
[jekyll-plugins]: http://jekyllrb.com/docs/plugins/
[jekyllthemes]: http://jekyllthemes.org
[howto]: http://davidensinger.com/2013/04/deploying-jekyll-to-github-pages/

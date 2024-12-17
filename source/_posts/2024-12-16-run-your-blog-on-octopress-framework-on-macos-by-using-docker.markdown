---
layout: post
title: "Run your blog on Octopress framework on MacOS by using Docker "
date: 2024-12-16 19:41:57 +0000
comments: true
author: Yevgen Voronetski
categories: english octopress docker macos dependencies blog 
---


{% img flotte thumbnail /images/octopress_logo.png 100 100 'Octopress on docker' %}

You are running your blog on **Octopress** framework.
You have trouble to make it work after each major OS update because of **dependencies hell**.
The solution is to use **Docker image**.

<!-- more -->

# Context
I started to use Octopress in 2016. At that time everything worked like a charm. 
In 2024 the Octopress is no longer supported as I see in https://github.com/octopress/octopress (last commit was 9 years ago).
And after each major OS update (I run MacOS) I have troubles with octopress installation, mainly because of **ruby** and **system dependencies** updates.
{% pullquote %}
Surround your paragraph with the pull quote tags. Then when you come to
the text you want to pull, {" surround it like this "} and that's all there is to it.
{% endpullquote %}
❗Everything was tested on **MacOS Sequoia** (15.1.1 )

# Problem
Obviously, we need to use a Docker. 

⚠️But I could not make work the official docker octopress image https://hub.docker.com/r/octopress/octopress

# Solution

I crafted specific [Docker image](https://hub.docker.com/r/yevgune/octopress_ruby) to save my work and hours of fixing annoying dependency problems.
I hope this work will be useful for other people as well.

You just need to run this command from your octopress root directory in your favourite terminal:
```  
docker run -v $(pwd):/srv -p 4000:4000 -it yevgune/octopress_ruby:5 bundle exec jekyll serve --host=0.0.0.0 
```

Now you can go to your host system, open your favourite browser at http://localhost:4000 and you will get 
the Octopress running locally. You change the post/page and octopress wrapped by Docker will regenerate the 
site. That is it. 🥳

## Publishing with Rsync 🔄

I'm using the **rsync** method to deploy site on remote server (go prod).
And there is some 🧙 **magic parameters** for Docker to securely **forward** your ssh-agent socket to container 
```
docker run --rm  -v $(pwd):/srv -v /run/host-services/ssh-auth.sock:/run/host-services/ssh-auth.sock -e SSH_AUTH_SOCK="/run/host-services/ssh-auth.sock"   -p 4000:4000   -it yevgune/octopress_ruby:5
```
and once in container, you can run the rsync :
```
bundle exec rake rsync
```














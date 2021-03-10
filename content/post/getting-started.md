+++
title = "Getting Started"
date = "2021-03-09"
author = "Sean"
cover = "img/getting-started/getting-started-cover.png"
coverAlt = "git clone of Sean Jones app in a terminal"
description = "The first step towards documenting a journey, creating a space to document"
+++

The current goal is to use as many free public cloud offerings as possible to roll as many “self hosted” solutions as possible while paying as little money as possible.

## The original plan

So before I started building this lab and documenting my progress on it I decided I would use GitHub Pages for hosting my site. There were a few reasons for this:

1) I already have a GitHub account
2) Hosting a GitHub Pages site is free
3) Setting up my custom domain is easy
4) SSL certs are provided for free as well
5) I've used it before briefly so was familiar with the process

Since I had setup a simple site using this method before I was able to get my quick, single page, demo site up and running with my domain name in a couple minutes. The longest part was waiting for DNS to propogate so GitHub would apply the certificate to my page.

{{< image src="/img/getting-started/getting-started-01.png" alt="Hello Friend" position="center" style="border-radius: 8px;" >}}

## Where things went wrong

Anyone who's worked in IT knows that the original plan for a rollout will need some adjusting and changing somewhere along the way. For this project, the first change was immediately.

I used the [Jekyll Docker Image](https://github.com/envygeeks/jekyll-docker/blob/master/README.md) instead of a native install sine I don't use Ruby for anything else. I've used this on my Linux desktops before without issue so assumed that the same process would work as expected for my new Mac machine.

I was able to scaffold the new site with the Jekyll image without issue. The first problem arose once I tried to serve my site locally. As soon as I kicked this off it failed with the following error:

```sh
vscode ➜ /workspaces/seandjones92.github.io (initial-setup) $ bundle exec jekyll serve --livereload
Configuration file: /workspaces/seandjones92.github.io/_config.yml
            Source: /workspaces/seandjones92.github.io
       Destination: /workspaces/seandjones92.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.992 seconds.
                    ------------------------------------------------
      Jekyll 4.2.0   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
Traceback (most recent call last):
        43: from /usr/local/bundle/bin/bundle:23:in `<main>'
...
...
         4: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/base.rb:42:in `each'
         3: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/base.rb:47:in `block in configure'
         2: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/linux.rb:34:in `_configure'
         1: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/linux.rb:34:in `new'
/usr/local/lib/ruby/gems/2.7.0/gems/rb-inotify-0.10.1/lib/rb-inotify/notifier.rb:69:in `initialize': Function not implemented - Failed to initialize inotify (Errno::ENOSYS)
```

Since the error referenced `inotify` I assumed it was because I had not configured `/proc/sys/fs/inotify/max_user_instances` to allow enough instances, which I had seen before a while back and has a pretty straight forward fix. I was having some trouble finding out how to modify this setting on the Docker VM for Docker Desktop for Mac.

I turned to the [Docker Desktop](https://www.docker.com/docker-community) Slack channel where I found I was not the only one having trouble with Jekyll and the new M1 macs. As it turns out there seems to be a C extention for the `inotify` gem that doesn't really work with the M1 Macs yet.

## Thinking outside the box

Since the ability to serve the project locally was broken I decided to try using Visual Studio Code to connect to a remote development machine. Since my goal is to leverage as many free cloud services as I can I decided to spin up an `f1-micro` instance on GCP.

Google Cloud offers this small VM image to be run perpetually for free. That plus it's location in the cloud seemed to be the perfect combo. Since the build environment wouldn't be local at least in a public cloud it would be highly available to me.

Spinning up the machine, adding my public SSH key, and connecting to it with the VSCode SSH plugin worked without issue. The problem with this method arose once the VSCode Server and the Jekyll tooling exhausted the 0.6 GB of RAM that comes with the `f1-micro` instance.

## How to keep moving forward

Instead of fighting with this much more I decided to try finding a site generator that _would_ work with my machine. I landed on Hugo since it seemed like a straight forward and simple solution and was built with Golang, which as of version 1.16 now supports the M1 chips.

While Hugo isn't something I have much experience with the learning curve wasn't too rough and it was still just as free to use as Jekyll. The next step was to replace GitHub pages hosting which provided a free way to host my site. For Hugo to be a valid replacement I would also need a way to host this site and apply my domain name for free.

After some research ([and seeing that Hugo provides docs on how to do it](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)) I landed on Netifly. It has a free tier which includes "serverless functions" which GitHub does not. So already it's a better value for someone trying to build a cloud based lab while spending as little cash as possible 👍

## Where I go from here

The next post will go over what it was like for me to get this site up and running with my custom domain and SSL on Netifly. I'll also do some initial exploration of the netifly platform as it will all be very new for me!
